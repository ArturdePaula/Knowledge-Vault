# O que é? 

Registry é uma aplicação server-side que armazena e distribui imagens de contêineres Docker. Normalmente, durante o uso do docker, caso não haja nenhuma configuração, as imagens docker são sempre puxadas do Registry DockerHub, que é o padrão. 

Usar um registry pessoal possibilita o mesmo nível de versionamento e controle das imagens docker, que repositório Git entrega para as mesmas finalidades em código. Melhor que um registry, é se for um privado acima de tudo, on-premisses. Isso garante total controle sobre o versionamento das imagens, backup e controle de acesso customizado. 
# Configuração docker compose padrão

```yaml
services:
  registry:
    image: registry:3
    container_name: REGISTRY-DOCKER
    restart: unless-stopped
    ports:
      - "5000:5000"
    volumes:
      - /var/lib/registry:/var/lib/registry
      # Volume com os dados de autenticação
      - ./auth:/auth:ro
      # Volume com o certificado e senha para TLS, se utilizável diretamente
      - path/certs:/cert
    environment:
      # Para alterar a porta padrão do registry, caso usar TLS nele diretamente
      # altere a porta para 443
      - REGISTRY_HTTP_ADDR=0.0.0.0:443
      # Variáveis para configurar usuário e senha para o registry
      - REGISTRY_AUTH=htpasswd
      - REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm
      - REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd
      # Variáveis para configurar TLS no registry
      - REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt
      - REGISTRY_HTTP_TLS_KEY=/certs/domain.key
```
# Tageando as imagens docker

Para fazer o processo de push de uma imagem para o registry, é necessário colocar nelas uma tag, para que o docker identifique que estamos realizando um push para nosso registry privado. Para isso é necessário colocar um prefixo de "endereço:porta", ou se caso houver DNS, somente o dominio é necessário, dessa forma, o docker vai entender que é o endereço do registry para onde estamos enviando a imagem. Um exemplo usando a imagem do ubuntu, e um registry local

```shell
# Puxando a imagem do repositório
docker pull ubuntu:latest

# Retagendo a imagem para envio ao registry
docker tag ubuntu:latest localhost:5000/ubuntu:latest

# Enviando a imagem ao repositorio registry privado
docker push localhost:5000/ubuntu:latest

# Realizando o pull da imagem customizada do registry privado
docker pull localhost:5000/ubuntu:latest
```
# Usando o docker registry

Para usar o docker registry, precisamos primeiro logar no registry usando a operação "login", se o registry tiver usuário e senha, será requisitado que você forneça essas informações para realizar o login:

```bash
docker login registry.example.com.br
```

Após o login, realize o build da imagem, seguindo a nomeclatura padrão informada anteriormente, ou então, taggeando a imagem para ser enviada ao registry e realize finalmente a operação de push, como explicado anteriormente também. 

# Criando usuário e senha para o docker registry 

O padrão para ambientes de produção para o registry, é que ele seja protegido por usuário e senha, para criar um usuário e senha padrão, vamos usar um serviço de htpasswd da imagem do httpd, por padrão, rode esse comando onde o arquivo ficará alocado na maquina. Ele vai criar um arquivo chamado htpasswd, contendo o usuário e senha hasheado por bcrypt

```shell
docker run --entrypoint htpasswd httpd:2 -Bbn usuario_registry 'senha_registry' > auth/htpasswd
```

Após isso, mapeie o arquivo para dentro do container, conforme o arquivo de compose padrão. 
# Habilitando garbage collect

Por padrão, quando uma imagem é excluida do docker registry, o que acontece realmente é a exclusão do arquivo de manifest da imagem, que contem as informações gerais de como aquela imagem é construida, o que impossibilita o download da mesma, mas o restante dos arquivos - **blobs e layers da imagem** - não é excluida por padrão no momento da exclusão do manifest, sendo necessário executar o garbage collect do registry manualmente. 

> [!NOTE]- Arquivos excluidos
>  **Camadas (Layers) de Imagem Não Referenciadas:** As imagens Docker são compostas por camadas (layers). Quando você exclui uma _tag_ ou um _manifesto_ de imagem (o que é o primeiro passo para "excluir" uma imagem), as camadas associadas podem ficar sem referência. Se ela não é usada por mais **nenhuma** imagem, deve ser excluida para liberar espaço
>  
>   **Manifestos Não Referenciados:** Manifestos são arquivos que descrevem uma imagem e listam quais camadas (blobs) ela usa. Ao excluir a referência de uma imagem (por exemplo, a tag `meu-repo:latest`), você remove o manifesto.
>    
>   **Outros Blobs "Órfãos":** Isso pode incluir metadados ou outros arquivos que, por algum motivo, foram desvinculados de um manifesto ativo.

Em um ambiente de produção, é necessário colocar o comando do garbage collect para executar automaticamente uma vez por dia ao menos, para realizar a limpeza do arquivos que ficam para trás após a "exclusão" de uma imagem: 

```shell
# O comando padrão para executar o garbage-collect no registry
docker exec container_registry registry garbage-collect --delete-untagged /etc/distribution/config.yml
```

Coloque este comando para executar te tempo em tempo no cron do servidor, ou em qualquer outra ferramenta para execução de scripts/comandos instalada no servidor.

# Configurando o docker client para aceitar um registry "não seguro"

Caso seu certificado para o registry seja autoassinado, é necessário cadastrar o certificado no client do docker no servidor que vai realizar as operações de pull e push. Para fazer isso, tenha o arquivo .crt em mãos e então execute os comados abaixo. **Lembre de modificar o dominio do registry quando executar o primeiro comando**: 

```shell
sudo mkdir -p /etc/docker/certs.d/dominio_registry                 

sudo mv ./registry.crt /etc/docker/certs.d/dominio_registry/ca.crt
```
 