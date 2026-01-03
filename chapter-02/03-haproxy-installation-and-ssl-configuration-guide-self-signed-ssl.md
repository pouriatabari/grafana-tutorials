# HAProxy Installation and SSL Configuration Guide (Self-Signed SSL)

This document provides a **step-by-step guide** to install, configure, and secure **HAProxy** on a fresh Linux server. The HAProxy instance will act as a **dedicated load balancer / reverse proxy** and will use a **self-signed SSL certificate** for the domain:

```
tosinso.local
```

---

## Table of Contents

1. Prerequisites
2. System Preparation
3. Install HAProxy
4. Enable and Verify HAProxy Service
5. Generate a Self-Signed SSL Certificate
6. Configure HAProxy (HTTP + HTTPS)
7. Validate Configuration
8. Restart and Test HAProxy
9. Common Troubleshooting
10. Directory Structure Summary

---

## 1. Prerequisites

* A fresh Linux server (Ubuntu 20.04 / 22.04 recommended)
* Root or sudo access
* HAProxy used **only** as a load balancer (no application workloads)

---

## 2. System Preparation

Update the system packages:

```bash
sudo apt update && sudo apt upgrade -y
```

Install required utilities:

```bash
sudo apt install -y curl gnupg2 ca-certificates openssl
```

---

## 3. Install HAProxy

Install HAProxy from the official Ubuntu repository:

```bash
sudo apt install -y haproxy
```

Verify installation:

```bash
haproxy -v
```

Expected output should show the HAProxy version.

---

## 4. Enable and Verify HAProxy Service

Enable HAProxy to start on boot:

```bash
sudo systemctl enable haproxy
```

Check service status:

```bash
sudo systemctl status haproxy
```

---

## 5. Generate a Self-Signed SSL Certificate

### 5.1 Create SSL Directory

```bash
sudo mkdir -p /etc/haproxy/ssl
cd /etc/haproxy/ssl
```

### 5.2 Generate Private Key and Certificate

```bash
sudo openssl req -x509 -nodes -days 365 \
  -newkey rsa:2048 \
  -keyout tosinso.local.key \
  -out tosinso.local.crt
```

**Common Name (CN):**

```
tosinso.local
```

### 5.3 Combine Certificate and Key (HAProxy Requirement)

```bash
sudo cat tosinso.local.crt tosinso.local.key > tosinso.local.pem
sudo chmod 600 tosinso.local.pem
```

---

## 6. Configure HAProxy

### 6.1 Backup Default Configuration

```bash
sudo cp /etc/haproxy/haproxy.cfg /etc/haproxy/haproxy.cfg.bak
```

### 6.2 Edit HAProxy Configuration

```bash
sudo nano /etc/haproxy/haproxy.cfg
```

### 6.3 Sample HAProxy Configuration

```cfg
global
    log /dev/log local0
    log /dev/log local1 notice
    chroot /var/lib/haproxy
    stats socket /run/haproxy/admin.sock mode 660 level admin
    user haproxy
    group haproxy
    daemon

    ssl-default-bind-options ssl-min-ver TLSv1.2
    ssl-default-bind-ciphers PROFILE=SYSTEM

defaults
    log     global
    mode    http
    option  httplog
    option  dontlognull
    timeout connect 5s
    timeout client  50s
    timeout server  50s

frontend http_front
    bind *:80
    redirect scheme https if !{ ssl_fc }

frontend https_front
    bind *:443 ssl crt /etc/haproxy/ssl/tosinso.local.pem
    default_backend app_backend

backend app_backend
    balance roundrobin
    server app1 127.0.0.1:8080 check
```

> Replace backend servers as needed for your environment.

---

## 7. Validate Configuration

Before restarting HAProxy, validate the configuration:

```bash
sudo haproxy -c -f /etc/haproxy/haproxy.cfg
```

Expected output:

```
Configuration file is valid
```

---

## 8. Restart and Test HAProxy

Restart HAProxy:

```bash
sudo systemctl restart haproxy
```

Check listening ports:

```bash
ss -tulnp | grep haproxy
```

Test using curl:

```bash
curl -k https://tosinso.local
```

> `-k` is required because the certificate is self-signed.

---

## 9. Common Troubleshooting

### HAProxy fails to start

Check logs:

```bash
journalctl -u haproxy -xe
```

### SSL not working

* Ensure `.pem` file contains **both certificate and private key**
* Check file permissions

```bash
ls -l /etc/haproxy/ssl/
```

---

## 10. Directory Structure Summary

```
/etc/haproxy/
├── haproxy.cfg
├── haproxy.cfg.bak
└── ssl/
    ├── tosinso.local.crt
    ├── tosinso.local.key
    └── tosinso.local.pem
```

---

## Conclusion

You have successfully:

* Installed HAProxy on a dedicated server
* Configured HTTP to HTTPS redirection
* Secured traffic using a self-signed SSL certificate
* Prepared a clean, production-style HAProxy configuration

This document is ready to be published on **GitHub** as a complete HAProxy setup guide.

