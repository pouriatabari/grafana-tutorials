# Installing Grafana with Docker

This guide provides two methods to install Grafana with Docker:

1. Simple execution with `docker run`
2. Execution with Docker Compose

---

## 1. Installing Grafana with a single `docker run` command

First, make sure Docker is installed on your system:

```bash
docker --version
```

Then, run Grafana with a single command:

```bash
docker run -d \
  -p 3000:3000 \
  --name=grafana \
  -e "GF_SECURITY_ADMIN_PASSWORD=admin" \
  grafana/grafana:latest
```

* `-d` → run the container in the background
* `-p 3000:3000` → map port 3000 on the host
* `--name=grafana` → name the container
* `GF_SECURITY_ADMIN_PASSWORD=admin` → initial admin password

After running the container, open Grafana in your browser:

```
http://localhost:3000
```

Default username and password:

```
User: admin
Password: admin
```

---

## 2. Installing Grafana with Docker Compose

### Step 1: Create the `docker-compose.yml` file

In your project directory, create a file named `docker-compose.yml`:

```yaml
version: "3.8"

services:
  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=admin
    volumes:
      - grafana-data:/var/lib/grafana
    restart: unless-stopped

volumes:
  grafana-data:
```

### Step 2: Run Docker Compose

```bash
docker-compose up -d
```

This command:

* Starts the Grafana container
* Stores data in the `grafana-data` volume
* Ensures the container restarts automatically

### Step 3: Access Grafana

Open Grafana in your browser:

```
http://localhost:3000
```

Default username and password:

```
User: admin
Password: admin
```

---

## 💡 Important Notes

* On first login, Grafana will prompt you to change the password
* To access Grafana remotely, make sure port 3000 is open in your firewall
* Docker Compose is useful for managing multiple services and persistent data

