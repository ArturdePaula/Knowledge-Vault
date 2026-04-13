## Dockerfile

O seguinte Dockerfile é apenas um exemplo de como ficaria uma montagem de uma aplicação, no caso usamos uma baseada em FastAPI, o comando final que estamos executando é o seguinte: 

O comando final, para executar o servidor esta configurado para rodar na porta 15400, utilizando 2 workers Uvicorn , trabalhando com logs de acess padrão e com logs de error no nível de info capturando também o stdout, mantendo conexões abertas por 60 segundos, tendo um timeout para requisições de 180 segundos : 

```dockerfile
# Usando a imagem do Alpine por ser menor
FROM python:3.10.14-alpine3.20

# Definindo um diretório de trabalho
WORKDIR /app

# Instalando o fast, uvicorn e gunicorn
RUN pip3 install fastapi uvicorn gunicorn

# Setando o container para o horário do brasil, importante para códigos sensíveis a horário
ENV TZ=America/Sao_Paulo

# Código em python, portanto, instalando os requirements
COPY ./requirements.txt /app
RUN pip install -r requirements.txt

# Carregando o restante dos arquivos necessários para o código executar, cada estrutura de códigos é unico, então copie os necessários no seu projeto
COPY ./main.py /app
COPY ./.env /app
COPY ./apis /app/apis

# Criando uma pasta para alocar os logs que vão ser gerados pela aplicação, no caso, é necessário caso venhamos a exportar os logs para um agregador
RUN mkdir -p logs

# Expondo a porta padrão do FastAPI no container
EXPOSE 15400

# Aqui, damos o comando para iniciar o servidor com as configurações, pode ser alterado conforme o necessário
CMD ["gunicorn", "main:app", "--worker-class", "uvicorn.workers.UvicornWorker", "--bind", "0.0.0.0:15400", "--workers", "2", "--log-level", "info", "--access-logfile", "logs/access.log", "--error-logfile", "logs/error.log","--capture-output", "--enable-stdio-inheritance", "--keep-alive", "60", "--timeout", "180"]
```

## Compose

O documento de compose.yaml do docker fica configurado aproximadamente desta forma : 

```yaml
services:
  api:
    build: .
    container_name: "API"
    ports:
      - "8200:15400"
    restart: always
    networks:
      - api_network
    deploy:
      resources:
        limits:
          cpus: "3"
          memory: "2g"

    volumes:
      - logs:/logs/

volumes:
  logs:

networks:
  api_network:
    driver: bridge
```