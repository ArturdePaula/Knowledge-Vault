Para alcançar a máxima performance no N8N, alem de escalonamento horizontal facilitado e métricas de limites bem definidos, precisamos usar o modo "queue".

## Modo Queue

É um modo de configuração e instânciamento do N8N que permite que tenhamos um servidor "main", que se concentra nas tarefas de login, criação de novos fluxos, configuração geral da aplicação e processsamento de webhooks, ficando o restante do processamento dos fluxos em N servidores "workers". Isso, alem de colocar um limite mais claro de processamento simultâneo, também deixa o restante da instância livre, isolando o processamento em cada worker. Vejamos como isso funciona: 

- A chamada do fluxo é realizada no servidor main, que registra a chamada da execução em uma fila de execução no servidor redis. 
- O servidor do redis é responsável por manter uma fila de execuções pendentes e controlar as que já foram executadas das que não foram ainda, e quais estão alocadas já em algum worker
- Os servidores workers ficam monitorando a fila junto do redis para procurar por execuções pendentes na fila, se acharem, eles pegar a execução, avisam ao redis que estão trabalhando nela, e assim que o resultado for finalizado, eles enviam o resultado novamente ao redis, que envia também novamente ao servidor main
- Caso um worker falhe em completar a tarefa por qualquer motivo que seja, ele devolve a requisição a lista pendente de execução, para que outro worker pegue

![[Untitled-2026-01-07-1335.png]]

## Vantagens

- **Escalabilidade Horizontal**: Permite uma abordagem de escalabilidade do serviço de forma horizontal que é mais benéfica para o servidor e principalmente para o uso de docker e cluster de computadores. 
- **Isolamento de Falhas**: Permite que, ao ocorrerem falhas, elas fiquem isoladas em um "servidor" diferente dos demais, não compremetendo o funcionamento do restante do sistema 
- **Gerenciamento de carga**: O redis atua como um "amortecedor" de carga para as execuções, evitando que um unico servidor receba diversas execuções de uma só vez, o que poderia gerar engasgos e até mesmo falha critica, garantindo então que o servidor não se sufoque
- **Controle de limite e previsibilidade de escalabilidade:** Como os workers tem limites bem denifidos, é possível perceber com mais facilidade onde estão os limites de carga, o que ajuda em casos onde é necessário aumentar a capacidade de processamento. 
## Problemas resolvidos 

- Quando múltiplos fluxos eram executados simultâneamente, erros eram observados em alguns deles por conta da carga pesada de execução. 
- Durante uma carga pesada de execução, a interface web travava bastante e/ou ficava momentâneamente fora do ar, essa sendo a principal reclamação. 
- Quando pensávamos sobre escalabilidade do sistema, não havia uma definição clara de até onde era possível aumentar a "potência" do serviço até ele quebrar, no formato queue, sabemos com mais exatidão até onde o serviço consegue funcionar sem quebrar
## Observações

Para que os workers e a main executem corretamente empatados, garanta que: 

- Todos operam na mesma versão do N8N
- Todos acessem o mesmo banco de dados
- Todos compartilhem o mesmo volume de dados
- Todos compartilhem a mesma chave de encriptação para o banco de dados
- Todos compartilhem o mesmo modo de salvamento de arquivos (Default = filesystem) 
#### Como fazer as instâncias compartilharem a mesma chave de encriptação

Caso a instância seja nova, você mesmo pode gerar a chave, e então passa-la em todas os containers, seja da main ou dos workers: 

```yml
# Gere a chave de encriptação
openssl rand -hex 24

# Passe a chave nos containers usando a variável de ambiente
environment: 
	- N8N_ENCRYPTION_KEY=chave
```

Caso a instância já existia, e você esta convertendo a mesma para performance: 

```yml
# Pegue a chave que foi gerada de dentro do container em: 
/home/node/.n8n/config

# Passe a chave nos containers usando a variável de ambiente
environment: 
	- N8N_ENCRYPTION_KEY=chave
```
#### Como fazer o N8N executar como worker? 

Basta apenas colocar no container do worker a seguinte linha no arquivo de compose.yml: 

```yml
command: worker
```
#### Como fazer com que as instâncias de main e worker entrem em queue mode? 

Basta adicionar as seguintes variáveis de ambiente nos containers main e worker, alem de uma instância do redis para que elas usem

```yml
environment: 
	# Configuração de Queue Mode
	- EXECUTIONS_MODE=queue
	- QUEUE_BULL_REDIS_HOST=redis
	- QUEUE_BULL_REDIS_PORT=6379
```

#### Como modificar a quantidade de processos simultâneos que cada worker pode trabalhar

Nos containers dos workers, é possível definir a quantidade de execuções simultâneas por worker: 

```yaml
environment: 
	- N8N_WORKERS_CONCURRENCY=quantidade desejada
```
