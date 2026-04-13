### Funções 

Funções em Go são definidas pela sintaxe, qualquer coisa colocada entre as chaves é considerado como escopo da função.

```Go
func main () {
  codigo_que_será_executado
}
```

### Packages (Pacotes) 

Pacotes em GO são requisitos funcionais da linguagem que precisam existir, eles são declarados no começo de um arquivo go para dizer que aquele arquivo em questão pertence aquele pacote. 
O código em GO precisa ser divido em pacotes, visando primeiro organização e segundo visando funcionando quando for realizado o build do código para um executável. 

```Go
package nome_do_pacote
```

> [!tip] Pacote main
> 
> Para o código funcionar, é necessário que o pacote principal, onde o código realmente será executado, se chame main. Isso vai funcionar para indicar que aquele arquivo é o ponto de entrada padrão do código que esta sendo executado quando rodarmos o comando go build
> 
> Para mais informações dos comandos vá até [[Comandos GO]]
### Módulos

Módulos em Go é um conceito que diz respeito a um grupo de pacotes, ou seja, uma aplicação. Podemos dizer que cada modulo é uma aplicação formada por vários pacotes, e é necessário que o modulo seja instanciado para que então um executável seja criado. Veja sobre os comandos aqui: [[Comandos GO]]
### Declarando variáveis

É possível declarar varáveis em go de varias formas **diferentes**.  Aqui vão algumas dessas formas: 

```Go
// Dessa forma, declaramos de forma tipada, informando que é uma variável e tipando o tipo logo em seguida também
var nome_variavel {int,float64,string ...}

// É possível também declarar uma variável mas sem a tipagem
var nome_variavel

// Mas o habitual para não tipado é a nomeclatura ":=" que deixa ao GO a decisão de qual é o tipo da variável
nome_variavel := valor

// A declaração de um valor constante segue as mesmas regras, mas não é possível fazer com ":=", visto que é necessário avisar que é uma constante
const nome_variável
const nome_variável {int,float64,string ...}
```