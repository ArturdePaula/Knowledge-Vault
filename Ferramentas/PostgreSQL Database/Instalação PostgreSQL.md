Para instalar o PostgreSQL na maquina host, seguimos os seguintes passos, o modo de instalação abaixo instala a versão do PostgreSQL que estiver disponível no repositório oficial de códigos da distribuição baseada em Debian que estiver em uso: 

```bash
# Atualiza o repositório da maquina
sudo apt update
```

```bash
# Instala o postgresql
sudo apt install postgresql postgresql-contrib
```

```bash
# Verifique se o banco de dados esta online
systemctl status postgresql

# Caso não estiver
systemctl start postgresql

# Caso necessite restartar a aplicação
systemctl restart postgresql
```

#### Para instalar uma versão especifica do PostgreSQL

Caso a versão disponível no repositório oficial não seja adequada para o uso no momento, é possível instalar uma versão especifica seguindo os passos disponíveis no [site](https://www.postgresql.org/download/) oficial do PostgreSQL 

Após a instalação, precisaremos alterar alguns arquivos de configuração para permitir que algumas operações sejam feitas, seguimos então para a [[Configuração PostgreSQL]]