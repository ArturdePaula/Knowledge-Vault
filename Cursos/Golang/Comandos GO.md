
## go mod init

É o comando que diz ao Go para criar um modulo que reúna as dependências necessárias do projeto, juntamente com a versão do Go para que posteriormente seja criado um executável. 
É uma convenção de boa pratica, colocar o caminho do arquivo no git hub até seu projeto, para quando for publicado, ou um nome sugestivo interno para quando for algo interno para auxiliar na localização e entendimento do código a seguir. Essa instrução cria um arquivo chamado go.mod na raiz do pasta, onde estão registrados o nome do modulo, a versão do go e as dependências do projeto

```bash
go mod init nome_do_modulo

go mod init github.com/username/seu_projeto
```

Exemplo de arquivo no go.mod

```Go
module ibitelecom/api/omini

go 1.20 

require ( 
	github.com/gorilla/mux v1.8.0 
)
```
### go build

É o comando que diz ao GO para criar um executável que encapsula toda a execução do GO, esse executável pode ser executado sem ter o go instalado na maquina

```zsh
go build
```

