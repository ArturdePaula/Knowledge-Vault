# IP

O ip é uma identificação única , formata por 4 octetos de 8bits, ou seja, é um numero de 32 bits, ou seja, são usados 8 bits para formar cada valor dentro os 4 conjuntos existentes no valor do IP, por exemplo: 

> 10.101.113.68 

Todo equipamento ligado na rede precisa de um IP, lembrando que o IP descrito acima é um IPV4, hoje eu dia também temos o IPV6 que veio para substituir o IPV4
# Subnet

Uma subnet é uma subdivisão de uma rede em secções menores e isoladas ou não umas das outras, essas redes estão todas ligadas em um único roteador ou switch, seu formato padrão é de:

> 10.101.113.68/24

Para entender como funciona essa subdivisão, temos que analisar o CIDR, o valor que esta após a barra no IP, que dita quantos bits do ip estão destinados a rede. Analisamos da seguinte forma, pegamos a quantidade total de de bits de um IP, 32 diminuímos pela quantidade de CIDR ,  e a quantidade de bits que sobrar será o expoente da elevação de 2, então se temos 24, sobram 8 bits, 2 elevado a 8 é igual a 256, ou seja, podem ser distribuídos então 256 IP's na rede . 

# NAT ( Network Address Translation ) 

O serviço de NAT, é um processo que existe em aparelhos routers, que permite que seja usado apenas 1 IP externo para alimentar todo o tráfego de uma rede interna. Em suma, os passos são os seguintes:   

- A maquina dentro da LAN (Rede interna) tem um IP interno, ela faz uma requisição, enviando o pacote até o IP interno do roteador, o IP de gateway. 
- Ao chegar no roteador, ele modifica o source IP do header TCP para aportar para o IP externo do roteador, que é o IP que pode ser mapeado pela rede
- Deste IP de saída o pacote então é enviado para o destino
- O server envia a resposta até o router que faz o caminho inverso para entregar o pacote ao IP na rede interna que solicitou

Para que a NAT saiba como converter os endereços de placa e porta, existe dentro do router uma tabela de NAT, que contem a relação entre ip e porta interno e o IP e porta externo. 
# Firewall 

O firewall é um software ou então um appliance, ou seja, um hardware com o software embutido, que se posiciona entre a rede interna e a rede externa, a fim de proteger a rede interna de tráfego indesejado exterior. O firewall pode ser também uma maquina Linux, que pode usar o iptables para criar regras de acesso para os pacotes. A ideia geral do firewall é reduzir o risco de invasão, bloqueando as portas que não são usadas e expondo as que precisam.

É comumente usado com o conceito de Least Privilege, esse conceito consiste em dropar todos os pacotes que chegaram externamente e depois dar passagem apenas ao necessário.
# ARP

APR (Address Resolution Protocol) é um protocolo da camada de link mas que atua juntamente da camada de rede também. Ele permite que, existindo duas maquinas dentro da mesma subnet, possam se comunicar e servir de forma direta, sem passar por um router pelo caminho, essa comunicação é feita através de uma associação IP com o MAC address de cada equipamento: 

- O device que quer se comunicar envia um sinal a toda a rede, perguntando quem é o device com o IP tal e informando também seu MAC address.
- A maquina que tem este ID responde que é ela, avisando seu MAC também
- As informações são então salvas em uma tabela de ARP para comunicações futuras.

Essa ordem de eventos garante que a conexões sejam estabelecidas e que novas conexões futuras possam ser feitas de maneira mais rápida. 

# Bridge

Uma bridge, ou ponte em inglês é uma abstração da camada 2 que permite que duas interfaces se comuniquem diretamente sem passar por algum roteador, e mantendo a transparência entre os dois pontos que estão sendo interligados. No geral é uma forma de interligar dois pontos diretamente de duas redes diferentes diretamente sem a necessidade de meio termo entre as duas. 

Em containers, essa ideias são levadas a um nível mais isolado e granular. Toda rede bridge é isolada da rede do host e dos outros containers, além do que podemos ajustar a segurança de forma mais forte e ativa, visto que no host podemos apenas colocar regras de firewall. O isolamento e segurança dessas redes é o que mais garante a diferença







