O processo de boot diz respeito aos passos consecutivos que maquina precisa ter para iniciar corretamente. O passos em sí são os seguintes: 
### Power
É o passo inicial, onde clicamos no botão de "Power" no computador, energizando os componentes
### POST(Power On Self Teste)
Nesta etapa, todo o hardware é testado para verificar algum problema que o envolva diretamente. E nesta etapa onde ocorre os "bips", como 3 bips ou 2 significando alguma ocorrência no hardware
### BIOS/UEFI
Após os testes, entramos no firmware que esta diretamente localizado na placa mãe da maquina, na memória CMOS. A BIOS é o formato mais antigo, sendo que aparelhos mais modernos já vem com o UEFI no lugar, que conta com interface gráfica mais moderna, suporta discos maiores e tem funcionamento mais rápido, além de opções melhores de segurança como secure boot. A BIOS/UEFI contém a ordem de boot, ou seja, de qual dispositivo de memória vai ser puxado o bootloader 
### BootLoader (GRUB)
O Grub fica salvo dentro do device de armazenamento, podendo ser em um formato mais antigo de MBR, ou então no formato adotado recentemente GPT. O BootLoader é uma telinha onde é possível escolher qual sistema operacional será carregado, após a escolha, ele então carrega a Kernel na memória para iniciar o carregamento do sistema. É possível subir um sistema Linux com múltiplas Kernels por exemplo.
### Kernel 
O kernel tendo o controle, sendo um arquivo compactado normalmente, ele ainda não pode descompactar para iniciar o sistema, pois não tem acessos aos drivers ainda, para isso então, ela chama o Initrd 
### Initrd
O initrd é o responsável por armazenar os drivers necessário para que Kernel consiga montar o "/" corretamente, ele carrega todos esses drivers na memória e executa então o script init 
### Init/Systemd 
É o primeiro processo do sistema com PID 1, ele inicia então todos os outros processos no sistema. Após o carregamento dos outros processos corretamente, o sistema já esta pronto para uso
# Configurações do Grub

Grub, como vimos anteriormente, é a etapa que fazer a execução do Kernel e o init logo após para o boot do sistema, por consequência, geralmente é nessa etapa do boot que vamos resolver a maioria dos problemas. O arquivo de conf do Grub real fica na pasta /boot junto com os outros arquivos de boot, mas esse arquivo de conf nunca deve ser alterado manualmente, somente em casos de extrema emergência, para modificarmos efetivamente o grub, devemos modificar o arquivo localizado em /etc/default/grub e depois executar o comando para atualizar o Grub corretamente: 

```yaml 
grub-mkconfig -o /boot/grub/grub.cfg

# Caso seja o grub 2
grub2-mkconfig -o /boot/grub2/grub.cfg
```

Após as modificações, o grub vai compilar todas as modificações no arqui do grub no boot, salvando as configurações modificadas. Após isso, pode ser reiniciado o sistema para verificar as modificações no grub. Para mais informações a respeito do grub, podemos verificar o seu [manual](https://www.gnu.org/software/grub/manual/grub/html_node/Simple-configuration.html)

>[!tip] Dica importante para debbuging de boot
>
>Caso o boot estaja dando algum probema, você pode seguir os passos abaixo para conseguir iniciar o sistema: 
>
>- Durante o carregamento do Grup, pressione a letra 'e'
>- Adicione o comando 'rw init=/bin/bash' ao final da linha que começa com Linux, isso irá redefinir o comando de inicio do boot para um terminal bash com permissão de escrita
>- Com isso, bata realizar o necessário, como modificar a senha com o comando 'passwd'

# SysV ou SystemD

SysV ou "SysFive" foi o primeiro gerenciador de boot do Linux usado em larga escala, foi usado até o inicio massivo da utilização de processadores multinucleo e que podiam realizar paralelismo, isso porque, esse gerenciador não tem capacidade de multiprocessamento. Ele tinha diversos problemas que eram decorrentes de sua grande simplicidade e capacidades reduzidas, como gerenciamento de depêndencias e demais situação que hoje são normais de uso. 

Já o SystemD é mais moderno, já capaz de multiprocessamento, mas em contrapartida, muito maior e mais complexo do que o seu antecessor , agregando também gerenciamento de timezones, serviços, logs, rede e diversas outras aplicações. Substituiu praticamente completamente o seu antecessor em todas as distros modernas, mas ainda tem resistencias de admins mais antigos, por conta de sua complexidade. 
# RunLevel ou Target

RunLevel é um número inteiro que representa o nível de execução do sistema operacional no momento, foi bastante usado em distribuições mais antigas, mas caiu em desuso pela não centralização de informação, cada distro poderia ter atribuições diferentes para os mesmos números , o que dificultava seu uso de forma mais global. 

O target é o novo formato que veio posteriomente, juntamente do SystemD, que troca os valores inteiros por arquivos .target mais objetivos, que facilita seu uso. Isso não desabilitou a forma antiga de se lidar com os RunLevel, mas agora ele são apenas apontadores para os targets dentro do sistema, uma forma de se manter o padrão antigo também caso necessário para os usuários mais antigos. Os targets estão na pasta: /usr/lib/systemd/system

![[Run level.png]]
