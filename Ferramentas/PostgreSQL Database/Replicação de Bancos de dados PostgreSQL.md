
# Replicação Streaming

É uma forma de replicação nativa do Postgresql que usa a montagem master-standby. O servidor de bancos de dados que queremos replicar é o master, e o servidor que vai receber as atualizações é o standby. Nesse tipo de replicação o delay é de milissegundos a segundos, dependendo da latência da rede ou da carga de trabalho de ambos os bancos de dados. 

Nesse tipo de replicação, é necessário uma configuração do banco para que a replicação possa ser feita. 

A limitação nessa abordagem é que ele ocorre a nível de bloco de disco, ou seja, todas as tabelas de todos os bancos no servidor vão ser copiados, não sendo permitido uma granularidade  maior com relação ao controle do que se quer copiar. Por esse motivo é mais usada em situações de failover, backup em tempo real ou recuperação de desastres. 

Outra limitação, é que os bancos standby criados a partir dessa técnica são somente para leitura, ou seja, não é possível fazer atualizações somente no servidor standby. Isso pode ser uma vantagem e desvantagem, dependendo apenas da aplicação que será necessária. Mas é comumente usada para desviar toda operação de leitura para o standby. 
# Replicação Lógica

A replicação lógica é uma forma também nativa do Postgresql, mas que permite uma granularidade muito maior das tabelas e da forma como será feita a Replicação. 

Nessa forma de replicação, o tempo de replicação fica na casa dos milissegundos até segundos dependendo da latência da rede e carga de trabalho dos servidores. A configuração por si é moderada para cada conjunto de tabelas, mas deve ser replicada para cada banco de dados que se quer replicar, cada um tendo uma configuração própria 

Essa configuração não tem a limitação de que o banco standby não pode ser atualizado de forma separada do master, ambos os bancos podem receber atualizações de forma isolada. 

Uma coisa a se ficar atento em ambas as abordagens é que quando o banco master for excluído, o banco standby precisa ser reconfigurado para ser um "Master". 

## Passo a Passo para se realizar uma replicação Logica:

#### 1 - Habilite a replicação lógica no banco Master, colocando as demais linhas no arquivo postgresql.conf

```bash
wal_level = logical
max_replication_slots = quantidade_de_slots
max_wal_senders = quantidade_de_wal_senders
```
#### 2 - Configure o arquivo pg_hba.conf: 

```bash
host replication <nome do usuario de replicação que será criado> <ip_do_servidor_standby>/32 md5
```
#### 3 - Crie o usuário de replicação que será usado: 

```sql
CREATE USER <nome_usuário> REPLICATION LOGIN ENCRYPTED PASSWORD '<senha>';
```
#### 4 - Com o servidor configurado, reinicie: 

```bash
sudo systemctl restart postgresql
```
#### 5 - No banco de dados no servidor Master, criamos uma publicação, focando em algumas tabelas especificas ou em todas as tabelas do banco

```sql 
CREATE PUBLICATION <nome_publicacao> FOR TABLE tabela1, tabela2;
CREATE PUBLICATION <nome_publicacao> FOR ALL TABLES;

DROP PUBLICATION <nome_publicacao>;
```
#### 6 - No servidor standby, garanta que exista um banco com todas as tabelas

#### 7 - No servidor standby crie uma subscrição para receber os dados da publicação

```sql
CREATE SUBSCRIPTION my_subscription 
CONNECTION 'host=host_do_master dbname=nome_do_banco_master user=replicator password=password' 
PUBLICATION my_publication;
```
#### 8 - Vejamos agora alguns comandos para verificar o status da atualização

```sql
-- Verificar o status da publicação
SELECT pubname, pubowner::regrole, puballtables, pubinsert, pubupdate, pubdelete FROM pg_publication;

-- Para verificar o slots de publicação
SELECT * FROM pg_replication_slots;

-- Para retirar algum slot de publicação
SELECT pg_drop_replication_slot('slot_replicacao');
```

```sql
-- Para verificar o status da subscrição 
SELECT subid, subname, pid, relid, received_lsn, latest_end_lsn, latest_end_time
FROM pg_stat_subscription;

-- Para deletar um subscrition 
DROP SUBSCRIPTION <nome_da_subscription>;
DELETE FROM pg_subscription WHERE subname = <nome_da_subscription>;

-- Para verificar o status da subscrição por tabelas
SELECT sr.srrelid AS table_id, 
       r.relname AS table_name, 
       sr.srsubstate AS sync_state
FROM pg_subscription_rel sr
JOIN pg_class r ON sr.srrelid = r.oid
WHERE sr.srsubid = (SELECT oid FROM pg_subscription WHERE subname = 'nome_da_subscription');
```