É uma ferramenta usada para rastrear os routers da rede entre um cliente e um servidor, mas principalmente, para identificar gargalos e latências muito altas que podem estar inviabilizando um contato correto com o servidor ou com o cliente. Usa por base o protocolo de transporte ICMP e para realizar as buscas, envia no cabeçalho da camada de rede o parâmetro TTL (Time to Live), incrementando em cada rodada para chegar até o final da rota ao destino. Vamos a alguns comandos:

```bash
# Para verificar todas as variáveis possíveis
mtr --help

mtr <ip ou dominio aonde se quer chegar>

# É possivel adicionar flags para mudar o pacote de ICMP para outros tipos
mtr --udp
mtr --tcp

# É possivel setar a porta tanto de saida quando de chegada
mtr --tcp --port <Porta> 
mtr --udp --port <Porta>
```


