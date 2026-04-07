# Telemt Stack
A production-ready Docker Compose configuration for the [Telemt](https://github.com/telemt/telemt) proxy server, integrated with a complete monitoring ecosystem (Prometheus, Grafana, and Node Exporter).

## Architecture
 - Telemt — is a fast, secure, and feature-rich server written in Rust: it fully implements the official Telegram proxy algo and adds many production-ready improvements.
 - Prometheus — Metric storage (isolated within the Docker network for security).
 - Grafana — Data visualization (the primary entry point for the user).
 - Node Exporter — Host system metrics (CPU, RAM, Disk).

## Prerequirements
 - Docker compose

## Quick Start
Clone the repository:
```bash
git clone git@github.com:trololobzic/telemt.git
cd telemt
```

Generate 32 hex chars secret:
```bash
openssl rand -hex 16
```
or:
```bash
xxd -l 16 -p /dev/urandom
```

Configure environment variables:
Copy the example file and set your preferred ports and credentials:
```bash
cp .env.example .env
cp telemt.toml.example telemt.toml
# Edit .env and telemt.toml with your favorite editor
# Note: The GF_SECURITY_ADMIN_PASSWORD variable in .env only sets the password during the initial container creation.
```

Launch the stack:
```bash
docker compose up -d
```

Get the link to your proxy:
```bash
docker compose logs telemt | grep "tg://proxy"
# change ip addres to your host real addres
```

## Metrics
By default, Prometheus and Node Exporter ports are not exposed to the public internet. Access to metrics is only available through the Grafana interface http://localhost:8888

## Maintenance & Troubleshooting
### Viewing Logs
To check the status of your services or debug issues, use the following commands:

View all logs:
```bash
docker-compose logs -f
```

View logs for a specific service (e.g., Telemt):
```bash
docker-compose logs -f telemt
```

### Resetting Grafana Password
If you forget your admin password, reset it using the Grafana CLI inside the container:
```bash
docker exec -it grafana grafana-cli admin reset-admin-password <new_password>
```

