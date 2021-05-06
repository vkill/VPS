## Consul

### Install

Ref https://www.atlantic.net/vps-hosting/how-to-install-consul-server-on-ubuntu-20-04/

Ref https://www.consul.io/downloads

```
wget https://releases.hashicorp.com/consul/1.9.4/consul_1.9.4_linux_amd64.zip
unzip consul_1.9.4_linux_amd64.zip
sudo mv consul /usr/local/bin/
consul -v
```

```
sudo adduser --system --group --no-create-home --home /var/lib/consul consul

sudo mkdir /var/lib/consul
sudo chown consul:consul /var/lib/consul
sudo chmod 750 /var/lib/consul

sudo mkdir /etc/consul.d
sudo chgrp consul /etc/consul.d

sudo vim /etc/default/consul
CONSUL_FLAGS="-dev -ui -client=0.0.0.0 -log-level err -data-dir /var/lib/consul"

sudo vim /lib/systemd/system/consul.service
[Unit]
Description=Consul Agent
Requires=network-online.target
After=network-online.target

[Service]
User=consul
Group=consul
EnvironmentFile=-/etc/default/consul
Restart=on-failure
ExecStart=/usr/local/bin/consul agent $CONSUL_FLAGS -config-dir=/etc/consul.d
ExecReload=/usr/bin/kill -HUP $MAINPID
KillSignal=SIGINT

[Install]
WantedBy=multi-user.target
```

```
sudo systemctl start consul
sudo systemctl enable consul
```
