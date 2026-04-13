Gunicorn e Uvicorn, são dois servidores baseados em python para aplicações em python. Os dois tem formas completamente diferentes de se trabalhar, mas podem e é sugerido fortemente que sejam usados de forma complementar, trabalhando juntos para servir aplicações diversas. Vamos a uma explicação rápida sobre os serviços. 
## Gunicorn

Gunicorn é um servidor WSGI, ou seja, trabalha de forma não assíncrona, ideal para servir paginas estáticas na WEB. É conhecido por ser bastante leve em uso de recurso e ser altamente veloz para gerir processos, pois trabalha com workers, fornecendo uma maneira mais robusta de se trabalhar com múltiplos núcleos da CPU, além de ter um sistema de autoexame, que pode reiniciar um worker quando acontece um problema com ele. 

Saiba mais na [documentação oficial](https://gunicorn.org/#quickstart)

## Uvicorn

Uvicorn é um servidor de ASGI, ou seja, ele trabalhar tanto com requisições não assíncronas como com requisições assíncronas, podendo trabalhar com aplicações que necessitam de múltiplos acessos, como API's. Seu sistema de trabalho não é tão robusto como é o caso do Gunicorn, em casos de falha, todo o sistema falha juntamente com ele, isso é comumente contornado criando varias instâncias do servidor em diferentes processos ou threads. Mas, o Uvicorn é reconhecido por ser um servidor extremamente eficiente e rápido. 

Saiba mais na [documentação oficial](https://www.uvicorn.org/)
## Combinados

Utilizando a capacidade de múltiplos processos do Gunicorn para instanciar processos Uvicorn, podemos utilizar as melhores capacidades dos dois mundos, ASGI e WSGI. O Gunicorn gerencia os acessos e distribui os processos entre os workers uvicorn, que processam a requisição de forma assíncrona. 

Essa modalidade permite que tenhamos um ambiente muito mais robusto, com autoexame e regulação e altamente escalável e estável. Compatível também com ambientes de produção utilizando outras ferramentas como proxy reverso NGINX e Apache. 

