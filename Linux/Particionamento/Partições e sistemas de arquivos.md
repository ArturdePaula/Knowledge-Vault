## Partições 

Partições são uma fração do disco rigido separada. Simples assim. No computador, podemos ter várias partições que dividem o disco em diversas partes, usadas para diversas finalidades diferentes. De forma nativa, o sistema operacional não consegue se comunicar com as partições se não houver um sistema de arquivos entre eles. Por isso que, ao criar partições, nós também instalamos nelas um sistema de arquivos para atender a finalidade que precisamos. 

Quais vantagens temos de separar os arquivos em partições? 

- Isolar partes importantes e/ou que tem taxa de escrita e leitura muito altas continuamente, para evitar que elas impactem o funcionamento de outras partes importantes do sistema operacional
- Em caso de corrupção ou falha, uma partição não infecta outra partição, permitindo que hava reparos para o sistema

## Sitema de arquivos

Um sistema de arquivos é a parte lógica que instalamos por cima de uma partição do disco rigido, que permite a utilização do sistema operacional naquela partição, podendo agora manipular dados dentro da partição (Armazenamento e organização dos dados) e também transferência entre partições. 

Cada sistema de arquivo tem uma forma de armazenar, organizar  e gerir os dados dentro da partição, isso significa que usar um sistema de arquivos mais antigo, vai impactar negativamente na experiência de uso, pois a gerência de dados será feito de forma possivelmente defasada. Alguns exemplos de sistemas de arquivos: 

- Ext2, Ext3 e Ext4
- XFS
- Btrfs

## Como as coisas se conectam

![[Pasted image 20250506112916.png]]

O disco rigido é montado no sistema em /dev/sda, cada disco novo recebendo uma letra ao final de sd, em ordem. Após o disco estar detectado, podemos então particiona-lo em partições menores conforme a necessidade, e instalar em cada uma delas um sistema de arquivos que atenda nossa demanda de uso. Após isso, difinimos um ponto de montagem, ou seja, qual caminho do sistema estará ligado aquela partição, montar o /home em /dev/sda3 por exemplo, nos indica que tudo que for colocado em /home vai ficar salvo nessa partição, completamente isolada das outras partições. 

## MBR ou GPT

MBR e GPT são definições de baixo nível para tabelas de partições que trabalham juntamente do disco rigido. Eles definem e armazeram informações como indices para que o disco rigido consiga encontrar informações dentro dele, chegar nas partições e todo o restante, mas essas definições também definem outros comportamentos que impactam no uso do disco pelo sistema operacional e nas operações. 

|                        MBR                        |                                             GPT                                             |
| :-----------------------------------------------: | :-----------------------------------------------------------------------------------------: |
|      Aceita no máximo 4 partições primárias       |                          Aceita no máximo 128 partições primárias                           |
|        Apenas 512 bytes para o bootloader         |                         Partições são nomeadas com GUID (ID unico)                          |
| Aceita discos de no máximo 2 terabytes de tamanho |                           Geralmente é usado juntamente com UEFI                            |
|                                                   | Automaticamente cria copias das <br>entradas do boot, impedindo trava por corrupção do boot |
|                                                   |  Tem proteção contra conversão <br>para MBR, para evitar apagamento da tabela de partição   |
