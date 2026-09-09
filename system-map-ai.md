# System Map (AI)

## 1. System Diagram
```mermaid
graph LR
    nginxClient[Nginx - client ingress]
    nginxApi[Nginx - API ingress]
    client[Client - React SPA]
    api[API - Django/DRF]
    monarch[Monarch - Python service]
    db[(Database - PostgreSQL)]
    valkey[(Valkey)]
    github[GitHub API]
    slack[Slack API]
    pgExporter[postgres_exporter]
    prometheus[Prometheus]
    grafana[Grafana]

    nginxClient -->|static serve :80/443| client
    nginxApi -->|HTTP proxy_pass :8000| api
    client -->|HTTP REST/JSON :8000| api
    api -->|SQL :5432| db
    api -->|pub/sub PUBLISH :6379| valkey
    api -->|cache GET/SET :6379| valkey
    api -->|HTTPS REST :443| github
    api -->|HTTPS REST :443| slack
    monarch -->|pub/sub SUBSCRIBE :6379| valkey
    monarch -->|HTTPS REST :443| github
    monarch -->|HTTPS REST :443| slack
    pgExporter -->|SQL query :5432| db
    prometheus -->|HTTP scrape :9187| pgExporter
    prometheus -->|HTTP scrape :8000| api
    grafana -->|HTTP query :9090| prometheus
```