O TCPDUMP é uma ferramenta Linux de terminal que funciona como o Wireshark, mas de visualização de terminal. Inclusive é possível salvar o monitoramento em um arquivo que pode ser aberto no Wireshark depois: 

```bash 
# Para monitorar o trafégo de uma interface em especifico
tcpdump -i <interface>

# Para monitorar o trafégo entre meu host e outro IP
tcpdump host <IP para monitorar>

# Para monitorar um IP de saida
tcpdump src <IP para monitorar>

# Para monitorar um IP de entrada
tcpdump dst <IP para monitorar>

# Para salvar o trafégo em um documento que pode ser lido pelo wireshar
tcpdump -w <nome_do_arquivo>.pcap
```