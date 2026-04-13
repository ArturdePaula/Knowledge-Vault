O docker tem 3 opções para criação de uma Networking. A opção de network do Docker é uma abstração da ferramenta em cima de diversas operações mais complexas executando por baixo. Um exemplo seria a criação de uma bridge, que envolve a comunicação com interfaces de rede de forma complexa entre namespaces do Linux. Algumas opções disponíveis para uma rede docker incluem  Para o padrão o comando para criar uma network foram de um container é o seguinte:

```bash
# O comanbdo basico é o abaixo, que cria uma rede to tipo bridge por padrão
docker network create nome_da_rede

# Para especificar o tipo de rede que será criada, usamos
docker network create nome_da_rede --drive tipo_da_rede
docker network create nome_da_rede -d tipo_da_rede

# Temos diversas outras variáveis para serem colocados no momento da criação de uma rede para visualizar
docker network --help
```

Caso você precise fazer isso a partir do script de um docker compose, dois passos serão necessários, usaremos de exemplo um docker compose para subir um container nginx. Você precisa primeiro dentro do service identificar a(s) networks que vão ser utilizadas, e se alguma network for declarada no próprio arquivo do compose, precisa ser declarada abaixo, com o seu nome e seu driver. 

```yml 
services:
  nginx:
    image: nginx:latest
    container_name: nginx
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./conf.d:/etc/nginx/conf.d
    ports:
      - 80:80
    networks:
      - webnet
networks:
  webnet:
    driver: bridge
```

# Bridge

É um tipo de rede que serve para conectar dois containers ou mais em uma rede separada que permita a comunicação dos mesmos de forma isolada. A bridge é a rede padrão para criação no docker, conectando dois containers ou mais em uma subnet isolada de todas as outras. Dentro da bridge, os containers podem se comunicar diretamente chamando um ao outro pelo nome, pois o docker já acrescenta também uma resolução de nomes DNS. Essa característica de isolamento, permite criar camadas de comunicação entre containers que ajuda manter a segurança e divisão entre o que pode se comunicar com o que dentro da rede. 

Use esta rede como padrão para comunicação no geral salvo em casos específicos 

Para termos mais técnicos, toda vez que uma rede bridge é criada, o docker cria também uma interface de rede para essa rede nova. Cada container criado também tem uma interface de rede virtual, que conecta com a rede bridge e permite a comunicação. Todos esses passos são feitos na criação manual, mas totalmente abstraídos no ambiente docker 
# Host

A rede do tipo Host não é muito usual. Ela permite que o container rode nivelado na mesma rede do host. Enquanto uma rede do tipo bridge gera uma nova subnet, a rede do tipo Host coloca o container no mesmo nível do host, perdendo assim sua segurança de manter a rede isolada do restante. Deve ser usado em casos onde um requisito principal seja que a aplicação esteja na mesma rede do Host, e mesmo em casos assim deve ser usado com cautela. 
# Overlay

Uma rede Overlay é criada para comunicação entre containers de dois Host diferentes. É a rede padrão que é usada em Clusters, como o Docker Swarm e também o Kubernets. A rede do tipo Overlay é um tunelamento de dados, permitindo que sejam aplicados camadas de segurança e criptografia por cima, para garantir a segurança mais pesada da rede.


