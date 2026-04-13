O registry do docker por padrão não tem uma interface web para manutenção e afins. Caso queira, será necessário configurar uma UI separada para adminstrar visualmente o registry. Para isso vamos usar a interface joxit/docker-registry-ui, que é disponibilizada gratuitamente, também em imagem docker, que é a versão que vamos utilizar. 
# Compose padrão

> [!TIP] Dicas
> - **Se possível**, para facilitar a configuração, coloque os containers do registry e registry-ui na mesma network docker

```shell
  services:
	  registry-ui:
	    image: joxit/docker-registry-ui:main
	    restart: unless-stopped
	    ports:
	      - 9080:80
	    environment:
	      - SINGLE_REGISTRY=true
	      - REGISTRY_TITLE=Registry-UI
	      - DELETE_IMAGES=true
	      - SHOW_CONTENT_DIGEST=true
	      - NGINX_PROXY_PASS_URL=http://endereço_registry:5000
	      - SHOW_CATALOG_NB_TAGS=true
	      - CATALOG_MIN_BRANCHES=1
	      - CATALOG_MAX_BRANCHES=1
	      - TAGLIST_PAGE_SIZE=100
	      - REGISTRY_SECURED=false
	      - CATALOG_ELEMENTS_LIMIT=1000
	    container_name: REGISTRY-DOCKER-WEB
```

Alem do compose para a propria UI, também será necessário adicionar algumas variáveis no enviroment da imagem do registry docker que você tiver iniciado: 

```shell
# Variáveis para habilitar ui web
- REGISTRY_HTTP_HEADERS_Access-Control-Allow-Origin=[https://endereço_registry_ui]
- REGISTRY_HTTP_HEADERS_Access-Control-Allow-Methods=[HEAD,GET,OPTIONS,DELETE]
- REGISTRY_HTTP_HEADERS_Access-Control-Allow-Credentials=[true]
- REGISTRY_HTTP_HEADERS_Access-Control-Allow-Headers=[Authorization,Accept,Cache-Control]
- REGISTRY_HTTP_HEADERS_Access-Control-Expose-Headers=[Docker-Content-Digest]
```

Com essas configuraçõe setadas, entre no endereço do UI, se o seu registry tiver senha, para se logar aqui, será necessário fornececer as mesmas variáveis de login do seu registry padrão.