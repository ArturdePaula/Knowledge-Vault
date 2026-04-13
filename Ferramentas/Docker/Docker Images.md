
>[!tip]- Boas praticas na construção de imagens
>
>1. Mantenha os containers _ephemerals,_ isto é, que você possa parar, reiniciar e ele continue funcionando sem maiores problemas.
>   
>2. Seguir o princípio 6 do 12 factor app. O processo deve ser stateless, e qualquer persistência precisa ser feito em alguma aplicação que mantém o estado, mais comumente uma database.
  > 
>3. Não inclua arquivos desnecessários no seu Dockerfile, isso pode resultar em falhas de segurança e aumento no tamanho da imagem.
>
>4. Use .dockerignore para evitar "lixo" no Dockerfile.
>
>5. Use multi-staging para otimizar o tamanho da imagem quando possível
>
>6. Não instale pacotes desnecessários.
>
>7. Desacople as aplicações. Nunca usar um container para mais de um objetivo. Por exemplo, o Wordpress deve rodar em um container, enquanto a database roda em outro, nunca ambos em um container.
>
>8. Minimize o número de camadas, isso geralmente otimiza o tamanho da imagem.
>
>9. Use e abuse da camada de cache do Docker.

## Instruções de construção

Para se construir uma imagem, o primeiro passo é construir um Docker file, um arquivo que contem todas as instruções, em ordem, para que se construa o ambiente ideal para que a aplicação seja executada corretamente. Existem diversas instruções, que podem ser todas encontradas na [documentação](https://docs.docker.com/reference/dockerfile/#overview)
## Cache

O Docker tem um cache embutido na construção das imagens, como todo o processo é feito em camadas, o docker pode salvar em cache cada camada da imagem, ou seja, quando for necessário fazer um rebuild de imagem, o docker pula as partes que não foram alteradas até uma em especifico. A partir da primeira alteração que será necessário rodar novamente, todas abaixo precisaram também ser rodada novamente também. 

Uma dica para lidar com o cache do docker também é evitar "wildcards", ou seja, evitar usar comandos de copy que copiem todo o diretório de dados para dentro do container, porque qualquer mudança em qualquer arquivo vai dificultar a operação de cache no momento da Build novamente do código
## Trabalhando com variáveis

No docker, pode ser uma boa pratica ter variáveis importantes do código da aplicação passados por variável de ambiente, que podem ser passadas no momento da execução do container, não sendo necessário rebuildar a imagem para alterar alguns dessas variáveis. 
## Entrypoint

O comando Entrypoint muitas vezes pode ser confundido com a funcionalidade do comando CMD. Os dois comandos fazem algo parecido, eles executam um comando no container em funcionamento após sua criação. O que diferencia é que o comando CMD pode ser sobrescrito, o Entrypoint não pode. Isso significa que se os dois serem usados em conjunto, o comando do CMD vai ser como parâmetro para o Entrypoint. 

No geral, o comando Entrypoint pode ser usado como um comando para estabelecer a base do comando, e o CMD como acréscimo a esse comando, pode ser usado principalmente para "forçar" uma aplicação dentro do container. 

## Health Check

O health check serve para monitorar a aplicação a partir de dentro do container, ou seja, estabelecemos um comando para ser executado de tempos em tempos para garantir que o container esteja funcionando corretamente. Isso é bastante útil principalmente em ambientes de cluster, podendo configurar o Cluster para não direcionar o tráfego para um container que não esteja funcionando corretamente. 

Em ambientes que não são cluster, ainda pode ser útil se combinado com a instrução de restart, garantindo que se o container perceber que não esta funcionando bem, ele mesmo se derruba e se reconstrói, garantindo que o serviço continue funcionando corretamente 

Em aplicações de API, pode ser bom que a aplicação tenha um endpoint próprio para ser chamado e realizar o HealthCheck 
## .dockerignore

O .dockerignore é um arquivo que tem a mesma funcionalidade e finalidade do .gitignore no git hub, serve para você descrever quais arquivos você não quer copiar para dentro da sua imagem docker, mesmo que você não especifique exatamente os arquivos para copia dentro da imagem, ainda é importante utilizar o dockerignore para ter garantia absoluta que não vai ser copiado para dentro da imagem. 

Esse arquivo deve ficar junto do Dockerfile.

## Multi-Staging 

Multi-Staging é uma técnica para o build de imagem, que consiste em dividir o processo em uma estágio de build e outro de runtime. É bastante aconselhável em linguagens que são compiladas, ou seja, que utilizam compilador para gerar um binário que será executado como é o caso com Golang. Não é uma técnica que pode ser usada em linguagens interpretadas como Python. 

Um exemplo usando uma aplicação em Golang: 

```dockerfile
FROM golang:1.18.0-bullseye AS builder
WORKDIR /app
COPY app.go .
RUN go mod init main && \
  CGO_ENABLED=0 go build

FROM scratch
EXPOSE 80
COPY --from=builder /app/main /go/main
CMD ["/go/main"]
```
## Tags

Implantar Tags nas imagens docker que são enviadas ao registry do docker é uma tarefa que não pode ser negligenciada, visto que é dessa forma que garantimos que as imagens serão sempre identificadas com base em algo que queremos nos lembrar delas. Podemos usar o comando docker tag para adicionar uma nova tag a uma imagem :
 
```bash
docker tag imagem_local regristry/respositorio:tag 
```
## Vistoriando imagens

Em algumas ocasiões, será necessário revisar as camadas de uma imagem, para verificar comandos que foram feitos durante a construção ou para verificar as camadas para uma possível otimização. Nesses casos, podemos usar o seguinte comando e possiveis variações: 

```shell 
docker history id_nome_container
```

Para mostrar os comandos não truncados: 

```shell
docker history id_nome_container --no-trunc
```

Para visualização melhor, podemos destinar o arquivo até um explorador de arquivos

```shell
docker history id_nome_container --no-trunc | editor_de_texto -
```
## Salvando imagens em .tar para envio

Não é algo que deve ser muito usado, mas para casos de necessidade, é possível salvar uma imagem em um formato compactado .tar, para que seja enviado a outra pessoa. Para isso, usamos o comando, ele salvará a imagem no arquivo que passarmos para ele

```shell 
docker image save -o nome_do_arquivo.tar
```

Agora com o arquivo no computador de destino, podemos realizar o carregamento da imagem de forma que possa ser executada no host, para isso, podemos usar o comando: 

```shell
docker image load -i nome_do_arquivo.tar
```

