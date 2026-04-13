Para fazer login no banco de dados a partir do terminal, usamos:

```sql
psql -U nome_usuario -h endereco_host -p porta -d banco_de_dados_destino
```

Agora conectado no banco de dados, podemos utilizar alguns comandos diretamente no bash do Servidor, como:
	
```Shell
# Para listar todos os usuários no banco com seus respectivos atributos para o servidor
\du

# Para listar todas as tabelas no banco de dados no schema public ou em outros schemas
\dt
\dt <nome_do_schema>.*

# Para listar todas as views existentens no banco de dados
\dv

# Para listar todos os databases existentes no servidor
\l

# Para listar as permissões dos usuários sobre uma tabela do banco: 
\dp <nome da tabela ou view>

# Para listar todos os schemas em um banco 
\dn
```

Podemos também executar alguns comandos SQL diretamente neste terminal, vamos a alguns grupos de comandos: 

#### Comandos que envolvem usuários 

```sql
# Para dar permissão de leitura(SELECT), a uma determinada tabela do banco, usamos o comando abaixo: 
GRANT SELECT ON nome_tabela_ou_view TO nome_do_usuário

# Para alterar a senha de um usuário no banco de dados, use o comando abaixo:
ALTER USER postgres WITH ENCRYPTED PASSWORD 'nova_senha';

# Para alterar o username de um usuário
ALTER ROLE usuario_antigo RENAME TO usuario_novo;

# Para criar um usuário no banco de dados, pode usar o seguinte comando: 
CREATE USER username WITH ENCRYPTED PASSWORD 'password';

# Para excluir um usuário no banco de dados, podemos usar: 
DROP ROLE username;
```

#### Comandos que envolvem bancos de dados

```SQL
# Para criar um banco de dados use o comando: 
CREATE DATABASE dbname;

-- Para excluir um banco de dados use um dos dois comandos:
DROP DATABASE dbname;

-- Se caso existir conexão ainda aberta com o banco impedindo do DROP, use esse comando abaixo e depois o DROP novamente
SELECT pid, pg_terminate_backend(pid) FROM pg_stat_activity WHERE datname = 'gdsap_2_teste_faturas_totais' AND pid <> pg_backend_pid();
```

```Shell
# Para fazer um dump do banco de dados completo
pg_dump -U nome_usuario -W -h endereco_host -p porta -d banco_de_dados > arquivo_saida.sql

# Para fazer o dump do banco de dados com apenas a estrutura, usamos o comando
pg_dump -U nome_usuario -W -h endereco_host -p porta -d banco_de_dados --schema-only > arquivo_saida.sql

# Para fazer um dump caso o banco de dados não for da versão instalada na maquina:
docker run --rm -v $(pwd):/backup -e PGPASSWORD='your_password' postgres:16.3 pg_dump -U nome_usuario -h endereco_host -p porta -d banco_de_dados -f /backup/nome_do_dump.sql

# Para restaurar o dump de algum banco em outro banco, usamos o comando
psql -U nome_usuario -h endereco_host -p porta -d banco_de_dados_destino -f arquivo_dump.sql
```
#### Comandos que envolvem permissões

```sql
# Para dar privilégio de conexão a algum banco, use o comando: 
GRANT CONNECT ON DATABASE dbname TO username;

# Para dar privilégio de leitura a alguma tabela do banco, use os comandos abaixo a vontade: 
GRANT SELECT ON TABLE table_name TO username;
GRANT SELECT ON ALL TABLES IN SCHEMA schema_name TO username;

# Para dar privilegio de uso para outra schema ao não ser o publico:
GRANT USAGE ON SCHEMA auditoria TO seu_usuario;

# Para verificiar qual o privilégio um usuário especifico tem em uma determinada tabela, podemos usar o comando abaixo:
SELECT privilege_type 
FROM information_schema.table_privileges 
WHERE grantee = 'nome_do_usuario' AND table_name = 'nome_da_tabela';
```

#### Comandos para troubleshooting

```SQL
-- Comando para visualizar todos os processos ativos no banco, com seu PID
SELECT pid, query, state
FROM pg_stat_activity
WHERE state = 'active'

-- Comando para dropar um PID que estiver ativo 
SELECT pg_terminate_backend(pid);
```

#### Comando para verificar conexões abertas no momento com o servidor

```sql
SELECT 
    datname AS banco_de_dados,
    usename AS usuario,
    application_name AS aplicacao,
    client_addr AS ip_cliente,
    state AS estado,
    query AS ultima_query,
    backend_start AS inicio_conexao
FROM 
    pg_stat_activity
WHERE 
    state IS NOT NULL;  -- Filtra conexões ativas (não ociosas/inativas)
```


	