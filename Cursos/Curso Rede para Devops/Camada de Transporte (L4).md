A Camada de transporte, chamada também de camada L4 (Layer 4) é camada responsável por realizar  o transporte dos dados entre dois sockets (portas) de dois hosts diferentes, comumente o client e o server. 

Um socket é uma interface de comunicação de rede abstraída, ele serve como endpoint entre as duas maquinas que estão conectadas através do endereço conjunto de IP e porta. Sockets são servidos pelo Sistema Operacional para as aplicações que necessitarem comunicação em rede. 
Tecnicamente então, aplicações nos dois lados estão conversando primariamente com sockets, e não diretamente com a aplicação de destino. 

A camada de transporte então é a comunicação entre endereços de placa e porta do client e do server, entre dois processos em duas maquinas distintas, não deve ser confundido com a camada de rede (L3) que é responsável pela comunicação entre duas maquinas (IP - Host) através da rede passando por diversos roteadores. 

## TCP

O protocolo TCP é um protocolo de transporte que gera uma conexão para o envio de dados, diferente do protocolo UDP que apenas envia pacotes sem abrir uma conexão. O TCP também garante a entrega ordenada e completa de todos os pacotes ao destin+o, esse tipo de operação no entanto diminui a velocidade da transmissão de pacotes. 
### 3-way TCP Handshake

O Handshake TCP é uma sequencia de 3 trocas de informações entre cliente e servidor para garantir que os dois estão conectados e sincronizados, para receber e ordenar a informação corretamente. 

- **SYN**: O primeiro passo é o cliente enviando ao servidor uma requisição TCP com o parâmetro SYN =1, isso informa ao servidor que o cliente esta querendo começar uma conexão, ele também envia uma numero de sequencia aleatório ( Seq )que diz por onde os pacotes vão ser contados. 
- **SYNACK**: O server recebe um sinal SYN do cliente e retorna então o seu próprio SYN para o cliente cliente para sincronizar, e também envia um ACK, que contem o valor do no Seq do cliente +1, pedindo para que o cliente envie o próximo pacote da ordem 
- **ACK**: O cliente então responde ao servidor com um ACK que é o Seq que o server enviou + 1, informando qual o próximo pacote que o servidor deve enviar e também envia o SYN=0 para indicar ao servidor que a conexão foi estabelecida com sucesso. 






