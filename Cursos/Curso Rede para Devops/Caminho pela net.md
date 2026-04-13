Qualquer busca ou envio de dados na internet funciona com base na conexão entre um Client, sendo a maquina do usuário e o Server, sendo o servidor onde esta aplicação. No contexto, para cada processo ocorrer uma requisição entre as duas partes. Nesse contexto, os pacotes, como são chamados os segmentos de dados que correspondem a uma requisição HTTP(s) passam por diversos níveis que tem suas próprias funções. O protocolo HTTP forma a espinha de toda a net moderna.
## Camada de aplicação

Onde as informações são reunidas, as requests que serão enviadas até o server. Nessa camada são carregados os Payloads ( Dados ) que serão enviados até o server, gerados no protocolo HTTP

## Camada de transporte

É aqui que fica o TCP, protocolo de transporte padrão na internet. Ele vai segmentar o payload em pacotes menores, para facilitar o transporte. Além de encaixar no pacote a informação de qual porta no server os dados serão entregues e de qual porta no client a requisição saiu

## Camada de Rede

Nessa camada é encaixado no header do pacote o endereço IP onde o payload será entregue e de qual IP a requisição saiu

## Camada de Link

O pacote, chamado também de datagrama é transformado em um frame e então transportado nos cabos na ultima camada, a camada física