É uma ferramenta, parecida com o [[MTR]], mas exibe um pouco menos de informações e não é continuo, além de que o protocolo de transporte padrão é o UDP. Vamos aos comandos

```bash
# Para verificar todas as variáveis existentes
traceroute --help

# Comando padrão
traceroute <ip ou dominio>

# É possivel alterar o protocolo de envio para TCP e ICMP
traceroute -T
traceroute -I

# É possivel especificar a porta para o envio das requisições
traceroute -p=
traceroute --port=
```