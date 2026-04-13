Em versões anteriores a 18 do postgresql, na secção de volumes do docker, o caminho era: 

```yml
volumes: 
	- SEU-VOLUME:/var/lib/postgresql/data
```

Mas a partir da versão 18 do banco, para manter atualizaçẽos futuras mais faceis de serem feitas, para que o banco de dados mantenha duas versões funcionais do banco dentro de si enquanto a atualização é feita, os dados agora estão separados em pastas de versões, o volume de dados agora é montado dessa forma, para garantir que todas as versões, inclusive as posteriores também serão inclusas:

```yaml
volumes:
	- SEU-VOLUME:/var/lib/postgresql
```

## Atualização em volume existente

Para caso o seu banco de dados esteja salvando dados em um volume já existente, e você queira passar o banco para versões >= 18.0:

#### Tática do dump do banco

Essa ideia é mais simples, caso seja possível realizar um dump do banco, ou dos bancos de dados que você vai usar. 

- Realiza o dump dos bancos de dados do seu servidor
- Remova o container de banco de dados
- Limpe o volume do banco para zera-lo
- Inicie novamente o container com a nova versão do banco rodando
- Recrie o banco de dados dentro do servidor
- Suba o dump retirado anteriormente no novo banco de dados

Dessa forma, os dados vão se moldar no banco com a nova estrutura já criada na inicialização do servidor em docker

