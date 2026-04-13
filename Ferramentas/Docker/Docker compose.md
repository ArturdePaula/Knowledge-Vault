O docker compose é uma ferramenta para se trabalhar com containers, sendo do próprio docker. Ela é o primeiro estágio do que chamamos de infraestrutura como código, ou seja, nós condamos uma infra e passamos para ferramenta este código, que então executa essa instruções. 

No caso do docker compose, é uma ferramenta de orquestração de aplicações em containers. Nos codificamos um arquivo de configuração de formato yaml. Nesse arquivo colocamos instruções dos containers, imagens base, volumes, redes, dependências, limitações de ram e cpu e algumas mais configurações para que possamos subir toda uma aplicação em conjunto com seus determinados módulos e necessidades.

Todas as informações sobre usabilidade e funcionamento podem ser entendidas na documentação [oficial](https://docs.docker.com/compose/compose-file/) 

Mas como exemplo básico, temos o seguinte exemplo de docker compose: 

```yaml
services:
  api:
    build: .
    container_name: "API"
    ports:
      - "9000:15400"
    restart: unless-stopped
    networks:
      - omini_api
    deploy:
      resources:
        limits:
          cpus: "3"
          memory: "2g"
networks:
  omini_api:
    driver: bridge
```

No mais, o docker compose é uma poderosa ferramenta para facilitar o deploy de um ou mais containers juntos de uma mesma stack de aplicação. 