# Keyshell Payment App – Infrastructure (Docker Task 3)

## Run locally
    cd micro-services
    cp .env.example .env              # edit JWT_SECRET
    docker-compose up -d --build
    cd prometheus
    cp .env.example .env              # edit Grafana + SMTP values
    echo -n '<gmail-app-password>' > secrets/smtp_password
    docker-compose up -d

## Jenkins credentials to create
| ID | Kind |
|----|------|
| git-hook-token | Secret text (GitHub PAT) |
| nexus-creds | Username + password |
| dockerhub-creds | Username + password (use a Docker Hub access token) |
| sonar-url | Secret text |
| sonar-token-backend / sonar-token-frontend | Secret text |
| payment-app-env | Secret file (copy of `.env`) |
| monitoring-env | Secret file (copy of `prometheus/.env`) |
| smtp-password | Secret file (Gmail App Password) |

## Where each PDF issue is fixed (search for `FIX #n`)
| # | Issue | File(s) |
|---|-------|---------|
| 1 | Exposed secrets | `.gitignore`, `.env.example`, `docker-compose.yml`, `Jenkinsfile`, `prometheus/alertmanager.yml`, `prometheus/docker-compose.yml`, `prometheus/.env.example` |
| 2 | Hardcoded passwords in pipeline `environment` | `Jenkinsfile` |
| 3 | Health checks via public IP | `Jenkinsfile` (Backend Build) |
| 4 | Alertmanager / Grafana SMTP TLS | `prometheus/alertmanager.yml`, `prometheus/docker-compose.yml` |
| 5 | Missing node_exporter + alert_rules.yml | `prometheus/docker-compose.yml`, `prometheus/alert_rules.yml`, `prometheus/prometheus.yml` |
| 6 | json-file driver doesn't forward logs | `docker-compose.yml` (filebeat), `elk/filebeat/filebeat.yml`, `elk/logstash/config/logstash.conf` |
| 7 | `latest` image tags | `docker-compose.yml`, `prometheus/docker-compose.yml` |

## Not included
`backend/*/src`, `backend/*/requirements.txt`, `backend/common`, and the Angular source (`frontend/src`, `package.json`, etc.) come from your existing repo.
