# Homelab: Private Cloud and Observability environment with Docker

This repository contains the architecture and configuration of my local laboratory (Homelab). The objective of the project is to deploy a functional private cloud and a professional monitoring stack using containers, applying Infrastructure as Code (IaC) methodologies and security best practices.

---

## System Architecture
The environment is completely contanerized with Docker Compose and segmented into isolated networks to mitigate the attack surface

* **Storage**
  * **Nextcloud**
  * **PostgreSQL15** (for the NextCloud backend)

* **Observability and Monitoring**
  * **Node exporter** (for hardware metrics)
  * **Prometheus** (metric scraping)
  * **Grafana** (Visual Dashboard for the Prometheus database)

* **Security Practices**
1. **Network Isolation**: Configured two bridge networks (`net-frontend` and `net-backend`). Database and metric collection components operate isolated in the backend network.
2. **Secure Credential Injection**: Sensitive configuration managed locally usin a (`.env`) file.
3. **Principle of Least Privilege**: Monitoring containers have no access to or knowledge of the Nextcloud database credentials.

---

## Diagram
<p align="center">
  <img width="711" height="1021" alt="Homelab Architecture Diagram" src="https://github.com/user-attachments/assets/03970173-cdc3-4466-985e-aaa9fad48f03" />
</p>

---

## Deployment Instructions

### Prerequisites
* Docker and Docker Compose installed on the host system.
* Local user permissions configured in the `docker` group.

### Step 1: Clone the repository and initialize variables
Clone the project and create your local credentials file from the provided template:
```bash
git clone [https://github.com/PedroSaavedraR/Homelab.git](https://github.com/PedroSaavedraR/Homelab.git)
cd Homelab
cp .env.example .env
```
> [!NOTE]
> Edit the (`.env`) file with your own passwords

### Step 2: Assign permissions to the Grafana volume
For security reasons, the Grafana container runs under an unprivileged user (UID 472). It is necessary to grant ownership of the local persistence folder:
```bash
sudo chown -R 472:472 ./grafana_data
```
### Step 3: Launch the infrastructure
```bash
docker compose up -d
```

---

## Available Services
| Service | Local URL
| :--- | :--- |
| **Nextcloud** | `http://localhost:8080`
| **Grafana** | `http://localhost:3000`

> [!NOTE]
> To view system metrics immediately, log into Grafana with the credentials configured in your .env file, navigate to Dashboards -> Import, enter ID 1860 (Node Exporter Full), and bind it to the internal Prometheus data source.

