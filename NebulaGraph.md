## Nebula Graph

### Install v2

Ref https://github.com/bk-rs/nebula-rs/wiki/Install-Nebula-Graph-2.x-on-Ubuntu-20.04

```
wget https://github.com/vesoft-inc/nebula-graph/releases/download/v2.0.0/nebula-graph-2.0.0.ubuntu2004.amd64.deb

sudo dpkg -i nebula-graph-2.0.0.ubuntu2004.amd64.deb
```

```
wget https://github.com/vesoft-inc/nebula-console/releases/download/v2.0.0-ga/nebula-console-linux-amd64-v2.0.0-ga

sudo mv nebula-console-linux-amd64-v2.0.0-ga /usr/local/bin/nebula-console
sudo chmod +x /usr/local/bin/nebula-console
```

### Configurate v2

Ref https://github.com/bk-rs/nebula-rs/wiki/Configurate-Nebula-Graph-2.x

```
sudo apt install -y bc
```

```
sudo mkdir -p /data/nebula_data

cd /usr/local/nebula

sudo cp etc/nebula-storaged.conf.default etc/nebula-storaged.conf
sudo cp etc/nebula-metad.conf.default etc/nebula-metad.conf
sudo cp etc/nebula-graphd.conf.default etc/nebula-graphd.conf

sudo chmod 644 etc/nebula-storaged.conf
sudo chmod 644 etc/nebula-metad.conf
sudo chmod 644 etc/nebula-graphd.conf

sudo vim etc/nebula-storaged.conf

--data_path=/data/nebula_data/

sudo vim etc/nebula-metad.conf

--data_path=/data/nebula_data/

sudo vim etc/nebula-graphd.conf

--enable_authorize=true
```

```
sudo /usr/local/nebula/scripts/nebula.service start all

sudo netstat -tunlp | grep nebula
```

```
/usr/local/bin/nebula-console -u root -p nebula --addr=127.0.0.1 --port=9669

> CHANGE PASSWORD root FROM 'nebula' TO 'rootpass';

> CREATE USER myuser WITH PASSWORD 'mypass';

> CREATE SPACE myspace(partition_num=3, replica_factor=1);

> :sleep 3

> GRANT ROLE DBA ON myspace TO myuser;
```

```
/usr/local/bin/nebula-console -u myuser -p mypass --addr=127.0.0.1 --port=9669

> SHOW SPACES;
```

```
/usr/local/nebula/bin/db_dump --space_name=myspace --mode=stat --db_path=/data/nebula_data/nebula --meta_server=127.0.0.1:9559
```

### Tips v2

Reset

```
sudo /usr/local/nebula/scripts/nebula.service stop all

cd /usr/local/nebula
sudo rm -rf cluster.id
sudo rm -rf /data/nebula_data/nebula
sudo rm -rf data
sudo rm -rf logs
sudo rm -rf pids
```

### Build v1

Ref https://github.com/bk-rs/nebula-rs/wiki/Build-Nebula-Graph-1.x-on-Ubuntu-20.04

### Configurate v1

Ref https://github.com/bk-rs/nebula-rs/wiki/Configurate-Nebula-Graph-1.x
