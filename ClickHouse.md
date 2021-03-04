## ClickHouse

### Install Server

Ref https://clickhouse.tech/docs/en/getting-started/install/#from-tgz-archives

```
clickhouse_version=21.2.5.5

wget "https://github.com/ClickHouse/ClickHouse/releases/download/v${clickhouse_version}-stable/clickhouse-common-static-${clickhouse_version}.tgz"
tar -zxvf clickhouse-common-static-${clickhouse_version}.tgz
sudo clickhouse-common-static-${clickhouse_version}/install/doinst.sh

wget "https://github.com/ClickHouse/ClickHouse/releases/download/v${clickhouse_version}-stable/clickhouse-server-${clickhouse_version}.tgz"
tar -zxvf clickhouse-server-${clickhouse_version}.tgz
sudo clickhouse-server-${clickhouse_version}/install/doinst.sh
```

```
sudo adduser --system --group --no-create-home --home /var/lib/clickhouse clickhouse

sudo mkdir /var/lib/clickhouse
sudo chown clickhouse:clickhouse /var/lib/clickhouse
sudo chmod 750 /var/lib/clickhouse

sudo mkdir /var/log/clickhouse-server
sudo chown clickhouse:adm /var/log/clickhouse-server
sudo chmod 750 /var/log/clickhouse-server
sudo chmod g+s /var/log/clickhouse-server

sudo tee /etc/logrotate.d/clickhouse-server <<EOF >/dev/null
/var/log/clickhouse-server/clickhouse-server*.log {
    weekly
    missingok
    rotate 12
    compress
    notifempty
}
EOF

sudo systemctl enable clickhouse-server
```

```
sudo apt install -y xmlstarlet

sudo sed -i 's@^    <!-- <postgresql_port>9005</postgresql_port> -->$@    <postgresql_port>9005</postgresql_port>@' /etc/clickhouse-server/config.xml

# password is xxx
sudo xmlstarlet ed --ps -L -u //yandex/users/default/password -v xxx /etc/clickhouse-server/users.xml

sudo systemctl restart clickhouse-server

journalctl --no-pager -f -u clickhouse-server
```

### Install Client

```
clickhouse_version=21.2.5.5

wget "https://github.com/ClickHouse/ClickHouse/releases/download/v${clickhouse_version}-stable/clickhouse-client-${clickhouse_version}.tgz"
tar -zxvf clickhouse-client-${clickhouse_version}.tgz
sudo clickhouse-client-${clickhouse_version}/install/doinst.sh
```

```
# password is xxx

clickhouse-client --ask-password
```

### Clients

```
# password is xxx

mysql --protocol tcp -u default -P 9004 -p
psql -h 127.0.0.1 -p 9005 -U default
```
