## Shebang

É a primeira coisa que precisamos colocar no nosso código Shell, ele indica o caminho até o compilador do Shell que estamos usando. O padrão atual é o Bash, mas pode-se usar o Zsh também ou outro compilador. 

```Shell
#!/usr/bin/env bash
```
## Executando um script Shell

Para executar um código no Shell, devemos dar ao arquivo permissão de execução primeiro, para fazer isso digite o comando: 

```Shell
chmod +x arquivo
```

Após isso, é só executar o arquivo pelo terminal
## Variáveis 

As variáveis são declaradas de forma direta, a sintaxe delas fica da seguinte forma: 

```Shell
# Variáveis globais são feitas em maisculo e variáveis locais são em minusculo
NOME="Artur Oliveira de Paula"
nome="Artur Oliveira de Paula"

# Strings são colocadas em aspas duplas, e qualquer outro tipo é apenas o valor
NOME="Silas"
IDADE=24

# Podemos também passar para uma variável a saida de um comando, por exemplo
DOCKER="$(docker ps)"
```

## Condicionais com IF

