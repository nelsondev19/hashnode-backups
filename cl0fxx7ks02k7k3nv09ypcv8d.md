## How to monitor PosgreSQL with Prometheus and Grafana | Docker

1.Docker Compose

```yaml
version: "3.9"
services:
  grafana:
    image: grafana/grafana
    ports:
      - 3000:3000

  prometheus:
    image: prom/prometheus
    ports:
      - 9090:9090
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml:ro

  postgres:
    image: postgres:12
    ports:
      - 5432:5432
    volumes:
      - ./backup:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: postgrespassword
      POSTGRES_DB: shop

  postgres-exporter:
    image: prometheuscommunity/postgres-exporter
    ports:
      - 9187:9187
    environment:
      DATA_SOURCE_NAME: "postgresql://postgres:postgrespassword@postgres:5432/shop?sslmode=disable"
    links:
      - postgres
      - prometheus

```

2.Prometheus File Settings (prometheus.yml)

```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s

scrape_configs:
  - job_name: prometheus
    static_configs:
      - targets: ["localhost:9090"]
  - job_name: postgres-exporter
    static_configs:
      - targets: ["postgres-exporter:9187"]
```

3.Run Docker Compose

```bash
docker-compose up
```
4.Check status of Prometheus and PostgreSQL Exporter

```bash
http://localhost:9090/targets
```

5.Visit Grafana Dashboard

Default

User: admin
Password: admin

```
http://localhost:3000
```

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/0h3s7z7bk3zai5r7r47e.png)

6.Add data source

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/w71n8ehokbecgwdug55q.png)

Save and test


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5oqhx5d5fy1f7hfmrpon.png)

7.Import PostgreSQL Dashboard for Prometheus

For this we will use a Dashboard created by the community


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/r8w9t3kiwrccsrw8brcx.png)

https://grafana.com/grafana/dashboards/9628

8.Import JSON or code
![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/jt2nl7m16u5vs6acdhe8.png)


![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/sxay2jd25fgtgd2g9ix5.png)

Code: 9628

9.Dashboard

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/nd2njnq1ans9e1mtvyal.png)

Code of example (GitHub Gist)

https://gist.github.com/nelsoncode019/1c0f7930aab755223de7f971f3aca94b