## Promscale

### Install

Ref https://github.com/timescale/promscale/blob/master/docs/binary.md

```
curl -L -o promscale https://github.com/timescale/promscale/releases/download/0.2.1/promscale_0.2.1_Linux_x86_64

sudo mv promscale /usr/local/bin/

sudo chmod +x /usr/local/bin/promscale
```

```
sudo adduser --system --group --no-create-home --home /var/lib/promscale promscale

sudo mkdir /var/lib/promscale
sudo chown promscale:promscale /var/lib/promscale
sudo chmod 750 /var/lib/promscale

sudo vim /etc/default/promscale
PROMSCALE_ARGS=""

sudo vim /lib/systemd/system/promscale.service
[Unit]
Description=Promscale service
Requires=network-online.target
After=network-online.target

[Service]
User=promscale
Group=promscale
EnvironmentFile=-/etc/default/promscale
Restart=on-failure
ExecStart=/usr/local/bin/promscale $PROMSCALE_ARGS
ExecReload=/usr/bin/kill -HUP $MAINPID
KillSignal=SIGINT

[Install]
WantedBy=multi-user.target
```

### Configure

Require install TimescaleDB 2 with PostgreSQL 12

```
sudo su - postgres
psql -p 5433

postgres=# create database promscale_timescale;
postgres=# create user promscale with encrypted password 'xxx';
postgres=# grant all privileges on database promscale_timescale to promscale;

postgres=# ALTER USER promscale WITH CREATEROLE;
postgres=# ALTER DATABASE promscale_timescale OWNER TO promscale;

postgres=# \c promscale_timescale
promscale_timescale=# CREATE EXTENSION IF NOT EXISTS timescaledb;
```

```
psql -h 127.0.0.1 -p 5433 -U promscale promscale_timescale

promscale_timescale=> \dx
```

```
sudo vim /etc/default/promscale
PROMSCALE_ARGS="-db-host=127.0.0.1 -db-port=5433 -db-user=promscale -db-password=xxx -db-name=promscale_timescale"

sudo systemctl start promscale.service 
```
