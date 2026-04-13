## Grafana 

O Grafana é uma plataforma de visualização e análise de dados de código aberto. Sua principal utilidade é criar dashboards interativos e visualmente atraentes para monitorar e analisar dados de várias fontes. Vamos a alguns pontos-chave sobre o Grafana:

1. Visualização de dados: Permite criar gráficos, tabelas, mapas de calor e outros tipos de visualizações.
2. Integração de fontes de dados: Conecta-se a várias fontes de dados, como bancos de dados, serviços em nuvem e APIs.
3. Monitoramento em tempo real: Oferece atualizações em tempo real de métricas e logs.
4. Alertas: Permite configurar alertas baseados em condições específicas dos dados.
5. Personalização: Os dashboards são altamente personalizáveis para atender às necessidades específicas de cada usuário ou organização.
6. Colaboração: Facilita o compartilhamento de dashboards entre equipes.

Principais usos:

- Monitoramento de infraestrutura de TI
- Análise de desempenho de aplicações
- Visualização de métricas de negócios
- Monitoramento de IoT (Internet das Coisas)
- Análise de logs

Em ambientes com muitos alvos a monitorar, chamar diversas fontes de dados exige monitoramento constante e acaba sendo mais aberto a erros, para resolver o problema, podemos utilizar um agregador de métricas como o Prometheus. 

## Prometheus

Prometheus: O Prometheus é um sistema de monitoramento e alerta de código aberto, projetado para alta confiabilidade e escalabilidade. Principais características:

1. Coleta de métricas: Coleta métricas de sistemas e serviços em intervalos regulares.
2. Armazenamento: Armazena todos os dados coletados em um banco de dados de séries temporais.
3. Consulta: Usa PromQL (Prometheus Query Language) para consultar e agregar dados.
4. Alertas: Permite definir regras de alerta baseadas em métricas.
5. Service Discovery: Descobre automaticamente alvos para monitorar em ambientes dinâmicos.

## Utilizando os dois em conjunto

O Prometheus e o Grafana formam uma combinação poderosa para monitoramento e visualização:

1. Fonte de dados: O Grafana usa o Prometheus como uma fonte de dados, conectando-se diretamente ao seu banco de dados.
2. Visualização: O Grafana cria dashboards visualmente ricos a partir dos dados coletados pelo Prometheus.
3. Consultas: O Grafana permite usar PromQL para criar gráficos e painéis complexos.
4. Alertas: Ambos os sistemas podem gerenciar alertas, com o Grafana oferecendo uma interface mais amigável para configuração.
5. Histórico: O Prometheus fornece dados históricos que o Grafana pode usar para análises de tendências de longo prazo.
6. Escalabilidade: Esta combinação é altamente escalável, adequada para monitorar desde pequenas aplicações até grandes infraestruturas distribuídas.

Juntos, o Prometheus e o Grafana oferecem uma solução completa de monitoramento: o Prometheus coleta e armazena os dados, enquanto o Grafana os transforma em visualizações informativas e acionáveis.

Vejamos agora, com é possível colocar para funcionar uma stack padrão de Grafana + Prometheus usando docker e docker compose, usando também monitoramentos de helthcheck para saber 

```yml
services:
  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - 3000:3000
    volumes:
      - grafana-data:/var/lib/grafana
    networks:
      - granana-network
    restart: unless-stopped
    healthcheck:
      test: ["CMD-SHELL", "curl -f http://ip:porta/api/health"]
      interval: 20s
      timeout: 10s
      retries: 1
      start_period: 30s

  prometheus:
    image: prom/prometheus:latest
    container_name: prometheus
    ports:
      - 9090:9090
    volumes:
     - caminho_na_maquina/prometheus.yml:/etc/prometheus/prometheus.yml

    command: '--config.file=/etc/prometheus/prometheus.yml'
    networks:
      - granana-network
    restart: unless-stopped
    depends_on:
      - grafana 

volumes:
  grafana-data:

networks:
  granana-network:
    driver: bridge
```

