## Configurando atalhos para conexões SSH

Para não digitar os comandos repetidas vezes e automatizar o processo de conexão e login em servidores por SSH, o OpenSSH permite adicionar um arquvo de configuração dentro da pasta .ssh com as informações dos servidores alem de um apelido para chamada. 

É possível, mas opicional, colocar a senha também na configuração de login, em ambientes criticos ou sensíveis, pode não ser uma boa ideia fazer isso.

```shell
Host producao # Apelido da chamada
    Hostname 0.0.0.0 # IP ou DNS do servidor
    User producao # Usuário
    Port 22 # Porta SSH do servidor
    IdentityFile ~/.ssh/producao.pem # Arquivo pem contendo a senha ssh

Host homologacao
    Hostname 1.1.1.1
    User homologacao
    Port 22
```