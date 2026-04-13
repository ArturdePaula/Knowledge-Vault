## Restart Policies

Restart Policies são configurações que podemos colocar em um container docker para que eles reiniciem automaticamente sobre algumas condições. Pode ser atribuído usando uma flag "--restart" em um comando docker run e com "restart: policies" em um documento docker compose. Os tipos de restart policies são: 

- **on-failure**: O container irá reiniciar sempre que houver um erro durante sua execução que impeça a continuação da execução
- **unless-stopped**: Realiza o mesmo do on-failure mas também reinicia quando o processo do container acaba, sendo sucesso ou não
- **always**: Realiza o mesmo dos dois anteriores, mas também reinicia em casos de reinicio da maquina ou do daemon do docker 
## Docker Events

Docker Events são eventos do sistema docker, registrados pelo daemon, como alterações no funcionamento dos containers, execuções e paradas. É uma API que pode ser usada em projetos de monitoramento de ações, ou então de construção de um banco de dados com as informações do eventos para auditoria posterior. 