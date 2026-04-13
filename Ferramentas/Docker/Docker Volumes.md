
> [!tip] Em relação aos volumes: 
>
>"Um container docker foi feito para morrer"

Essa é a resolução máxima das aplicações feitas em container, mas isso também significa que se não houver uma solução prevista para a situação, os dados que estão sendo gerados pela aplicação no container serão perdidos assim que o container for derrubado, da mesma forma que funciona uma memória RAM em um computador. 

Para evitar que isso aconteça e que os dados sejam persistidos precisamos criar um volume para armazenar o que for gerado, para isso temos 3 principais métodos: 

## Bind Volume

É uma forma de criar um volume, ligando uma pasta ou arquivo do Host diretamente dentro do container. Nesta configuração, tudo que modificar na pasta no host vai ser transmitido para dentro do container e vice versa. É usada para passar configurações de ambientes e afins

```bash
caminho/no/host:caminho/no/container
```

## Local Volume

Mesma funcionalidade do Bind, mas agora ao invés de conectar um caminho no host até o container, o que fazemos é criar uma pasta, que fica sobre controle do docker, e todos os dados que o container gerar serão salvos nesse volume, que fica fora do container em uma pasta do Host. É usado para armazenarmos dados gerados na execução da aplicação dentro do container por exemplo.

```bash
nome-volume:caminho/no/container
```

## Distributed Volume

Um volume que é criado e distribuído por vários Hosts. É usado em ambientes Clustes, onde vários containers precisam acessar o mesmos dados, mas estão distribuídos em nodes diferentes do cluster 
