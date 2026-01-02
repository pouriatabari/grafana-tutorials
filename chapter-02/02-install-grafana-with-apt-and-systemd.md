# Installing Grafana with APT and systemd

This guide explains how to install Grafana on Ubuntu/Debian systems using the official APT repository and manage it with **systemd**. All steps are written from scratch and can be followed sequentially.

---

## Prerequisites

* Ubuntu or Debian-based Linux distribution (20.04 / 22.04 recommended)
* Root or sudo privileges
* Internet access

---

## Step 1: Update the system

```bash
sudo apt update
sudo apt upgrade -y
```

---

## Step 2: Install required dependencies

```bash
sudo apt install -y apt-transport-https software-properties-common wget
```

These packages are required to securely download packages from external repositories.

---

## Step 3: Add Grafana GPG key

Add the official Grafana signing key so APT can verify packages:

```bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
```

Verify the key was added successfully:

```bash
sudo apt-key list | grep Grafana
```

---

## Step 4: Add Grafana APT repository

Add the official Grafana OSS repository:

```bash
echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
```

Update package index:

```bash
sudo apt update
```

---

## Step 5: Install Grafana

```bash
sudo apt install grafana -y
```

This installs:

* Grafana binaries
* Default configuration files
* A systemd service unit

---

## Step 6: Manage Grafana with systemd

### Reload systemd (recommended)

```bash
sudo systemctl daemon-reload
```

### Enable Grafana to start on boot

```bash
sudo systemctl enable grafana-server
```

### Start Grafana service

```bash
sudo systemctl start grafana-server
```

### Check service status

```bash
sudo systemctl status grafana-server
```

You should see:

```
Active: active (running)
```

---

## Step 7: systemd service file (reference)

Grafana installs its service file automatically at:

```
/etc/systemd/system/grafana-server.service
```

Example content:

```ini
[Unit]
Description=Grafana instance
After=network.target

[Service]
Type=simple
User=grafana
Group=grafana
ExecStart=/usr/sbin/grafana-server \
  --config=/etc/grafana/grafana.ini \
  --pidfile=/var/run/grafana/grafana-server.pid \
  --packaging=deb \
  cfg:default.paths.logs=/var/log/grafana \
  cfg:default.paths.data=/var/lib/grafana \
  cfg:default.paths.plugins=/var/lib/grafana/plugins
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

⚠️ Normally, you do NOT need to edit this file.

---

## Step 8: Access Grafana

Open your browser and go to:

```
http://localhost:3000
```

Default credentials:

```
Username: admin
Password: admin
```

Grafana will ask you to change the password on first login.

---

## Step 9: Open firewall port (optional)

If UFW is enabled and you need remote access:

```bash
sudo ufw allow 3000/tcp
sudo ufw reload
```

---

## Step 10: Configuration file location

Main configuration file:

```
/etc/grafana/grafana.ini
```

After making changes, restart Grafana:

```bash
sudo systemctl restart grafana-server
```

---

## Summary

* Grafana installed via official APT repository
* Managed using systemd
* Service starts automatically on boot
* Accessible on port 3000

This method is recommended for **production environments** where Docker is not required.

