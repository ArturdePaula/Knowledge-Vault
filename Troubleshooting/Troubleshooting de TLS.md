Para realizar troubleshooting de TLS no geral, primeiro é necessário verificar qual o problema, podemos fazer isso através da Ferramenta [Wireshark](https://www.wireshark.org/)

Abaixo estão processos de Troubleshooting de TLS mais específicos
## Verificando o certificado do CA para adicionar no client

Para verificar o certificado de CA de uma pagina podemos usar o seguinte método usando a ferramenta OpenSSL, ela é uma ferramenta usada para praticamente tudo ao que se refere a certificado digitais SSL

Podemos verificar primeiro qual o certificado de CA que o servidor esta nos devolvendo com o seguinte comando, estamos especificando que queremos que ele se comporte como um client, que queremos que ele mostre o certificado do CA e que ele se conecte com a url que vamos passar em seguida. A porta da conexão pode ser 80 ou 443, no nosso caso vamos usar 443 por estarmos testando certificados

```bash
openssl s_client -showcerts -connect <url do site para conexão>:443
``` 

Essa opção retornará os certificados da pagina, pegue o certificado indica ser o do CA e então coloque o mesmo em um editor de texto e salve com a extensão .ctr. Assim que estiver salvo, clique duas vezes sobre o arquivo, ou então utilize o seguinte comando para verificar se é o certificado que você procura: 

```bash
openssl x509 -in </caminho/até/o/certificado.crt> -text -noout
```

Sabendo que esse é o certificado desejado do CA, basta adicionar este arquivo no seu client.