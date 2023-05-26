## PostgreSQL

### Install Common (Ubuntu)

https://wiki.postgresql.org/wiki/Apt

https://www.postgresql.org/download/linux/ubuntu/

```
sudo apt install -y curl ca-certificates gnupg

curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/apt.postgresql.org.gpg >/dev/null

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

sudo apt update
```

### Install Client (Ubuntu)

```
sudo apt install -y postgresql-client-15
```

### Install Server (Ubuntu)

```
sudo apt install -y postgresql-15

sudo sed -i -E "s/^[#]?listen_addresses = .*/listen_addresses = '0.0.0.0'/" /etc/postgresql/15/main/postgresql.conf
sudo sed -i -E "s/^[#]?port = .*/port = 5432/" /etc/postgresql/15/main/postgresql.conf
sudo sed -i -E "s/^[#]?max_connections = .*/max_connections = 1000/" /etc/postgresql/15/main/postgresql.conf

echo "host all all 0.0.0.0/0 scram-sha-256" | sudo tee -a /etc/postgresql/15/main/pg_hba.conf

sudo systemctl restart postgresql@15-main
```

```
sudo su - postgres
psql

postgres=# create database mydb;
postgres=# create user myuser with encrypted password 'mypass';
postgres=# grant all privileges on database mydb to myuser;
postgres=# \q

exit
```

```
psql -h 127.0.0.1 -p 5432 -U myuser mydb
```

### Server multiple instances (Ubuntu)

Ref https://www.percona.com/blog/2019/06/24/managing-multiple-postgresql-instances-on-ubuntu-debian/

```
pg_lsclusters
```

```
sudo pg_createcluster 15 xxx -d /var/lib/postgresql/15/xxx -p 15432

sudo systemctl start postgresql@15-xxx.service
```

```
sudo pg_dropcluster 15 xxx --stop
```

### Server database dir move (Ubuntu)

```
sudo systemctl stop postgresql@15-main
ps aux | grep -i '[p]ostgres'

sudo mkdir /media/data1/postgresql
sudo rsync -aqxP /var/lib/postgresql/ /media/data1/postgresql

sudo rm -rf /var/lib/postgresql
sudo ln -sf /media/data1/postgresql /var/lib/postgresql

sudo systemctl start postgresql@15-main
ps aux | grep -i '[p]ostgres'
```

### Install (CentOS 7)

https://www.postgresql.org/download/linux/redhat/

```
sudo yum install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm

sudo yum install -y postgresql13-server
sudo yum install -y postgresql13-contrib
```

```
sudo mkdir /path/pg_xxx
sudo chown postgres:postgres /path/pg_xxx
```

```
sudo su - postgres

/usr/pgsql-13/bin/pg_ctl init -D /path/pg_xxx

mkdir /path/pg_xxx/conf.d

echo "include_dir = 'conf.d'" | tee -a /path/pg_xxx/postgresql.conf

echo "listen_addresses = '0.0.0.0'" | tee -a /path/pg_xxx/conf.d/x.conf
echo "port = 5441" | tee -a /path/pg_xxx/conf.d/x.conf
echo "max_connections = 1000" | tee -a /path/pg_xxx/conf.d/x.conf

mv /path/pg_xxx/pg_hba.conf /path/pg_xxx/pg_hba.conf.OG
touch /path/pg_xxx/pg_hba.conf
echo 'host all all 0.0.0.0/0 md5' | tee -a /path/pg_xxx/pg_hba.conf

exit
```

```
sudo cp /usr/lib/systemd/system/postgresql-13.service /usr/lib/systemd/system/postgresql-13-xxx.service

sudo sed -i 's!Environment=PGDATA=/var/lib/pgsql/13/data/!Environment=PGDATA=/path/pg_xxx/!' /usr/lib/systemd/system/postgresql-13-xxx.service

sudo systemctl start postgresql-13-xxx
sudo systemctl status postgresql-13-xxx

sudo systemctl enable postgresql-13-xxx
```

```
sudo su - postgres

psql -p 5441
postgres=# create database mydb;
postgres=# create user myuser with encrypted password 'mypass';
postgres=# grant all privileges on database mydb to myuser;
postgres=# \q

exit
```

```
psql -h 127.0.0.1 -p 5441 -U myuser mydb
```

### Stats

https://www.postgresql.org/docs/current/monitoring-stats.html

```
SELECT datname, numbackends FROM pg_stat_database;
```

### Tips

```
sudo mkdir /etc/systemd/system/postgresql@.service.d
sudo vim /etc/systemd/system/postgresql@.service.d/restart.conf
[Service]
Restart=on-failure
```

* Count_estimate

https://wiki.postgresql.org/wiki/Count_estimate


