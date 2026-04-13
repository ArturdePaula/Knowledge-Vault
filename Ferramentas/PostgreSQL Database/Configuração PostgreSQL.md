## Configuração postgresql.conf

Configure o arquivo _**postgresql.conf**_ para receber requisições de qualquer IP, ele fica no caminho etc/PostgreSQL/<versão>/main/ na sua maquina Linux

> [!info]- Configuração postgresql.conf
> 
> ```bash listem adresses = ‘*’ ```

Ou então coloque o endereço de ip que quiser de forma especifica

## Configuração pg_hba_conf

Configure agora o arquivo _**pg_hba_conf,**_ e adicione as seguintes linhas:

Para garantir que qualquer ip será forçado a escrever sua senha para logar. O md5 é para configurar o padrão MD5 para logar, é feito com a senha do usuário em versões mais antigas do Postgresql, em versões mais recentes, utilize o método **‘scram-sha-256’**

> [!info]- Configuração pg_hba_conf
>
> ```bash host            all           all             0.0.0.0/0            md5```
> 
> ###### Adicione também a seguinte linha abaixo do dizer: **Database administrative login by Unix domain socket**
>  
>  ```bash local all postgres md5```

Após mudanças nesses arquivos, sempre reinicie o serviço do PostgreSQL:

```bash
sudo systemclt restart postgresql
```

Terminadas as configurações, discutiremos então os [[Comandos PostgreSQL]]