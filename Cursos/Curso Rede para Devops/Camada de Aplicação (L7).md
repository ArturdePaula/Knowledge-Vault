# Protocolo HTTP

No protocolo HTTP, existem as requests e as responses, requests para os clientes e responses para os servidores, ambos tem suas particularidades:

### Requests

Na request, o protocolo manda uma request line, e um header line. Na request line, vão informações do método que esta sendo utilizado, A URI, que é o que aonde você esta querendo alcançar e por fim a versão do protocolo HTTP utilizado .

```bash
 GET /home.html HTTP/1.1
```

Após isso vem as header lines, onde estão os dados de header que foram juntos na requisição, a parte realmente importante, pois manda informações que definem como a requisição será tratada pelo servidor:

```bash
Host: developer.mozilla.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: <https://developer.mozilla.org/testpage.html>
Connection: keep-alive
Upgrade-Insecure-Requests: 1
If-Modified-Since: Mon, 18 Jul 2016 02:36:04 GMT
If-None-Match: "c561c68d0ba92bbeb8b0fff2a9199f722e3a621a"
Cache-Control: max-age=0
```

### Response

Na response temos a reposta do server a requisição HTTP, ela vem com um formato parecido com a request. Primeiro temos a response line, quem vai conter informação da versão do HTTP utilizada, o status code da resposta , e a mensagem:

```bash
 200 OK
```

Logo após nós teremos os headers lines mais uma vez, trazendo informações sobre a response:

```bash
Access-Control-Allow-Origin: *
Connection: Keep-Alive
Content-Encoding: gzip
Content-Type: text/html; charset=utf-8
Date: Mon, 18 Jul 2016 16:06:00 GMT
Etag: "c561c68d0ba92bbeb8b0f612a9199f722e3a621a"
Keep-Alive: timeout=5, max=997
Last-Modified: Mon, 18 Jul 2016 02:36:04 GMT
Server: Apache
Set-Cookie: mykey=myvalue; expires=Mon, 17-Jul-2017 16:06:00 GMT; Max-Age=31449600; Path=/; secure
Transfer-Encoding: chunked
Vary: Cookie, Accept-Encoding
X-Backend-Server: developer2.webapp.scl3.mozilla.com
X-Cache-Info: not cacheable; meta data too large
X-kuma-revision: 1085259
x-frame-options: DENY
```

Por ultimo, na response também são entregues ao client o body, onde ficam os dados retornados.

## HTTP Connections

Nas versões anteriores do protocolo HTTP, era usado conexão não persistente, ou seja, para cada operação GET ou qualquer outra, precisava criar uma conexão TCP antes, resultando em 2 RTT (Round Trip Time) para cada instrução. Hoje em dia, usamos conexões persistentes, ou seja, no começo é criado uma conexão TCP e essa conexão é então reutilziada diversas vezes para as operações posteriores, diminuindo praticamente pela metade a quantidade de RTT para executar requisições

## Cookies

Cookies são variáveis que o servidor envia ao client, em sua grande maioria, com a finalidade de se “lembrar” dele. Um exemplo, você entra em um site e se loga, o servidor abre a sessão de autenticação, e devolve para o cliente alem da reposta da requisição um cookie de sessão, na proxima vez que o client for enviar uma requisição, ele envia também o cookie, não tendo então a necessidade de se autenticar novamente.

## Proxy

O proxy é o que chamamos de midleware, ou seja, ele fica no meio da conexão entre o server e o client. O proxy pode ser usado de diversas formas, duas das principais são, para filtrar o conteudo, podendo bloquear sites e conteudos desejados e também como cache na rede. No modo cache, toda requisição do client passa por ele, ele então faz uma requisição ao servidor, e do retorno ele armazena o header de ultima modificação, na proxima vez que o client fizer uma requisição para o mesmo conteudo, o proxy vai verificar no server, e pedir uma resposta de payload apenas de a data que ele tem tiver sido alterada, se não o server retornada uma requisição sem dados, que é extremamente rapida, o proxy então devolve os arquivos que ele tem salvo para o client

## API ( Application Programming Interface )

Uma API, é uma forma de empacotar regras de negócio de uma aplicação e coloca-la de forma amplamente disponível para todo tipo de acesso. Muito comum imagina-la como um porteiro, que recebe um pedido de informação, vai até a fonte e traz de volta para quem pediu. Muitas aplicações usam de API para consumir dados ou armazenar dados. Alem de ser uma forma de garantir a conformidade dos dados nas regras de negócio, também é uma forma de proteger o que quer que esteja “atrás” da API. Geralmente elas retornam dados no formato json, que permite que diversos tipos de aplicação realizar o consumo de dados de um mesmo lugar, sem maiores complicações

## Ferramenta Curl

O curl é uma ferramenta de linha de comando para realizar iterações com API’s e sites através de requisição HTTP(s), funciona muito bem e ajuda muito com testes e verificações

## DNS

DNS é um serviço que resolve nomes de dominio para IP. Digamos que vamos acessar o site do google, para a rede, o que importa é apenas o IP, então quando digitamos Google, fazemos uma requisição ao servidor de DNS, ele resolve o nome para o IP correto, devolvendo o mesmo, então a requisição para o numero de IP pode ser feita e o site encontrado. O DNS também acaba sendo usado para outras coisas, como host aliasing, mail server aliasing e como uma versão menos efetiva de um load balancer, mas que já ajuda bastate quando é a unica opção possível

### Estrutura do DNS

O DNS segue uma estrutura de arvore, por onde a requisição irá passar para encontrar o endereço de IP referente ao nome do domínio. Ao fazer a requisição, essa requisição vai até um dos 13 nós atuais de servidores denominados root serves, cada um sobre domínio de uma empresa diferente, esses servidores tem mapeados neles o servidores TLD, que são servidores onde se encontram os correspondentes de cada domínio, como .com, .net, .me, .br, .edu e por ai vai, esses servidores TLD então enviam a requisição para servidor Autoritativo, onde realmente estão os domínios, é esse servidor que responde o IP para a requisição do browser corretamente.

### DNS Records

São tipos de registros no arquivo de configuração do DNS que dita o que cada registro faz e significa.

## TLS

O TLS é um protocolo de conexão entre cliente e servidor em requisições HTTS, ou seja, conexões seguras, onde ocorre uma troca de certificados e chaves para garantir uma conexão segura. O estabelecimento da conexão se chama _“TLS Handshake”_ e segue os seguintes passos:

- Client Hello: O cliente inicia a troca enviando ao servidor informações a respeito das versões SSL/TLS que ele suporta e também quais Cypher ele suporta (Cypher Suite), normalmente em torno de 15 ou mais
- Server Hello: Recebendo as informações do client, o servidor analise e escolhe, dentre as varias opções fornecidas pelo client a mais segura disponível, enviando agora de volta ao client o seu certificado e as escolhas que ele fez de conexão. O client então valida a assinatura do certificado para atestar sua confiabilidade perante uma lista pré-determinada de confiança do client. Caso ocorra tudo bem a conexão é feita
- Authentication(Opcional): É uma etapa opcional a conexão onde o client também precisa fornecer ao server seu certificado e o processo é repetido de forma inversa.

Com a ferramenta Wireshark, é possível realizar o debug da rede durante o handshake do TLS, é possível acompanhar toda movimentação do TLS. 

## SSL Labs

É uma ferramenta para avaliar o status do SSL, certificados e Cypher de um determinado servidor, caso seja necessário fazer o troubleshooting de TSL de um servidor da qual não temos acesso diretamente. Pode ser acessado [aqui](https://www.ssllabs.com/ssltest/)
## Resolvendo problemas de TLS Handshake

Grande parte dos problemas encontrados com TLS são referidos a problemas com confiança de certificado, ou seja, o client não confia no certificado do Server. Em casos assim, para resolver nós precisamos importar no client o certificado do CA (Certificate Authority) que é a entidade que assina o certificado final. Tendo o certificado dela cadastrado, todo certificado assinado por ela vai ser aceito. O client no geral pode ser diversas coisas, desde o próprio Linux para a ferramenta curl, navegadores de internet que tem sua própria lista de certificado até aplicações em linguagens de programação como Java. 
O que importa é que a operação sempre será: Pegar o certificado do CA e adicionar esse certificado a lista de confiáveis da aplicação, você pode verificar mais no arquivo [[Troubleshooting de TLS]]





