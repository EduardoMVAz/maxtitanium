# Maxtitanium

Documentação do projeto maxtitanium, da matéria Plataformas, Microsserviços e APIs.

Colaboradores:
---
Eduardo Mendes Vaz
- Email Insper: eduardov1@al.insper.edu.br
- Github: [EduardoMVAz](https://github.com/EduardoMVAz)


João Lucas de Moraes Barros Cadorniga
- Email Insper: joaolmbc@al.insper.edu.br
- Github: [JoaoLucasMBC](https://github.com/JoaoLucasMBC)

<br>

Repositórios referentes à Entrega 1:
---
### Serviços sem persistência de Dados
- Serviço de Discovery: [maxtitanium.discovery](https://github.com/EduardoMVAz/maxtitanium.discovery)
- Serviço de Gateway: [maxtitanium.gateway](https://github.com/EduardoMVAz/maxtitanium.gateway)
- Serviço de Autenticação e Autorização: [maxtitanium.auth](https://github.com/JoaoLucasMBC/maxtitanium.auth) e [maxtitanium.auth-resource](https://github.com/JoaoLucasMBC/maxtitanium.auth-resource)
- Serviço de Gerenciamento do Docker: [maxtitanium.docker-api](https://github.com/JoaoLucasMBC/maxtitanium.docker-api)
### Serviços com persistência de Dados
- Serviço de Account: [maxtitanium.account](https://github.com/JoaoLucasMBC/maxtitanium.account) e [maxtitanium.account-resource](https://github.com/EduardoMVAz/maxtitanium.account-resource)
- Serviço de Product: [maxtitanium.product](https://github.com/JoaoLucasMBC/maxtitanium.product) e [maxtitanium.product-resource](https://github.com/JoaoLucasMBC/maxtitanium.product-resource)
- Serviço de Order: [maxtitanium.order](https://github.com/EduardoMVAz/maxtitanium.order) e [maxtitanium.order-resource](https://github.com/EduardoMVAz/maxtitanium.order-resource)
### Demais Exigências
- Monitoramento com Dashboard de microsserviços: Feito com Grafana
- Documentação das APIs padrão Swagger: Feito com SpringDoc
- **EXTRAS**: Tratamento de Erros e Caching com Redis em todos os Serviços
### Roteiro 2
- Serviço de Redis para Caching de Dados, feito pelo João Lucas Cadorniga

### Como Testar  
Foram incluídas Coleções do Thunder Client nesse repositório para testar as principais rotas dos microsserviços. Ao rodar o Serviço de Docker Compose, utilizar essas coleções para testar.

### Setup Prometheus  
Para que o prometheus funcione corretamente e colete os dados dos serviços, é preciso criar as pastas `/volume/prometheus` na raiz do serviço `maxtitanium.docker-api`. Então, copie o arquivo abaixo e cole em um arquivo `prometheus.yml` dentro da pasta:

```yaml
scrape_configs:

  - job_name: 'GatewayMetrics'
    metrics_path: '/gateway/actuator/prometheus'
    scrape_interval: 1s
    static_configs:
      - targets:
        - max-gateway:8080
        labels:
          application: 'Gateway Application'

  - job_name: 'AccountMetrics'
    metrics_path: '/accounts/actuator/prometheus'
    scrape_interval: 1s
    static_configs:
      - targets:
        - max-gateway:8080
        labels:
          application: 'Account Application'

  - job_name: 'AuthMetrics'
    metrics_path: '/auth/actuator/prometheus'
    scrape_interval: 1s
    static_configs:
      - targets:
        - max-gateway:8080
        labels:
          application: 'Auth Application'

  - job_name: 'ProductMetrics'
    metrics_path: '/products/actuator/prometheus'
    scrape_interval: 1s
    static_configs:
      - targets:
        - max-gateway:8080
        labels:
          application: 'Product Application'

  - job_name: 'OrderMetrics'
    metrics_path: '/orders/actuator/prometheus'
    scrape_interval: 1s
    static_configs:
      - targets:
        - max-gateway:8080
        labels:
          application: 'Order Application'
```

### Setup Grafana

Para o Grafana, também é preciso criar um arquivo de configuração no serviço `maxtitanium.docker-api`. Dessa vez, crie o arquivo `datasource.yml` na pasta `/volume/grafana/provisioning`:

```yaml
apiVersion: 1
datasources:
  - name: Prometheus
    type: prometheus
    access: proxy
    url: http://prometheus:9090
    isDefault: true
```

Por fim, importe o dashboard com ID `11378`, com o datasource do **prometheus**, e o dashboard de acompanhamento estará pronto com todos os serviços!

Repositórios referentes à Entrega 2:
---
O setup do Jenkins referente à Entrega 2 pode ser encontrado no seguinte repositório:
https://github.com/JoaoLucasMBC/maxtitanium.ops
