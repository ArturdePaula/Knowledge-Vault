O FHS ou File System Hierarchy é uma convenção para organização dos arquivos e diretórios dentro de um computador, o Linux por padrão usa um sistema de organização baseado nesse padrão, ficando dessa forma no final: 
![[linux-filesystem.png]]



> [!Tip] "Entra em root"
> "/" É o diretório raiz, também chamado de root, onde ficam todos os outros diretórios. Quando dizemos entra em root, ou entra no /, é aqui para onde você vai

### /bin: 
Onde ficam os binários, ou seja, os executáveis do sistema, seriam a mesma coisa do .exe no Windows. Todo programa instalado terá um arquivo aqui dentro, ou também os comandos padrões do Linux, como ls, cat e por ai vai
### /boot 
Aqui ficam os arquivos que dizem respeito ao boot do sistema, o arquivo do Kernel, do grub e outros
### /dev
Conhecido também como device. Aqui ficam a abstração em objeto de todas os devices que estão conectados no sistema, como discos, HD, mouse, teclado, impressora, pen-drives e afins 
### /etc 
Aqui ficam os arquivos de configuração das aplicações instaladas, como Google Chrome ou LibreOffice, cada aplicação tem uma pasta dedicada aqui dentro com seus arquivos de configuração
### /home 
Armazena os arquivos de usuário, cada usuário tendo uma pasta separada com seus downloads, arquivos no geral e todas as outras coisas pessoais de cada usuário individualmente
### /lib
Arquivos de biblioteca do Linux, como arquivos .so
### /media 
Diretório de montagem de um device com pen drive , CD ou HD externo. Quando espetamos o pen drive, ele cria uma abstração de objeto no /dev e monta ele aqui 
### /mnt 
Funciona com a mesma funcionalidade do /media, mas é temporária
### /opt 
Diretório Optional, pode ser usado para montar pacotes externos do Linux
### /root 
É o /home do usuário root
### /sbin 
Pasta de binários do sistema, aqui ficam os binários que só o root pode usar, binários para recuperação do sistema
### /srv 
Diretório de serviços. Usando geralmente por serviços no computador
### /tmp 
Pasta de arquivos temporários. Tudo que for apenas temporário no computador, pode ser montado nesta pasta
### /usr 
Aqui fica basicamente todas as pastas que existem fora, mas focadas todos no usuário especifico, então terá uma pasta para cada usuário, e dentro dela por exemplo temos uma /bin, onde ficaram os binários de aplicações instaladas apenas para aquele usuário especifico 
### /var 
Diretórios de variáveis, nele ficam os logs, cache dos sistema, spool (fila de processamento) para impressoras e afins, um /tmp também e por ai vai. Basicamente, tudo que não cair em nenhuma das outras pastas, cai nesta

