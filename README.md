## Install Alertmanager

```
wget https://github.com/prometheus/alertmanager/releases/download/v0.18.0/alertmanager-0.18.0.linux-amd64.tar.gz
```
```
## Extract the files from the archive.
```
```
tar xvzf alertmanager-0.18.0.linux-amd64.tar.gz
```
```
cd alertmanager-0.18.0.linux-amd64/
```
```
sudo mv amtool alertmanager /usr/local/bin
```
```
sudo mkdir -p /etc/alertmanager
```
```
sudo mv alertmanager.yml /etc/alertmanager
```
```
sudo mkdir -p /data/alertmanager
```
```
sudo useradd -rs /bin/false alertmanager
```
```
sudo chown alertmanager:alertmanager /usr/local/bin/amtool/usr/local/bin/alertmanager
```
```
sudo chown -R alertmanager:alertmanager/data/alertmanager /etc/alertmanager/*
```
```
cd /lib/systemd/system
```
```
## Create alertmanager.service file and add these configuration 
```
```
sudo touch alertmanager.service
```
```
alertmanager -h
```
```
sudo nano alertmanager.service
```
```
[Unit]
Description=Alert Manager
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=alertmanager
Group=alertmanager
ExecStart=/usr/local/bin/alertmanager \
  --config.file=/etc/alertmanager/alertmanager.yml \
  --storage.path=/data/alertmanager

Restart=always

[Install]
WantedBy=multi-user.target
```
```
sudo systemctl enable alertmanager
```
```
sudo systemctl start alertmanager
```
```
Binding AlertManager with Prometheus
cd /etc/prometheus/prometheus.yml


alerting:
  alertmanagers:
  - static_configs:
    - targets:
      - localhost:9093


