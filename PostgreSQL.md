## PostgreSQL

### Install Common

https://wiki.postgresql.org/wiki/Apt

```
sudo apt-get install -y curl ca-certificates gnupg

curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

sudo apt-get update
```

### Install Client

```
sudo apt install -y postgresql-client-13
```

### Install Server

```
sudo apt install -y postgresql-13

sudo vim /etc/postgresql/13/main/conf.d/listen_addresses.conf
listen_addresses = '0.0.0.0'

sudo vim /etc/postgresql/13/main/pg_hba.conf
host all all 0.0.0.0/0 md5

sudo vim /etc/postgresql/13/main/conf.d/max_connections.conf
max_connections = 2000

sudo systemctl restart postgresql
```

```
sudo su - postgres
psql

postgres=# create database mydb;
postgres=# create user myuser with encrypted password 'mypass';
postgres=# grant all privileges on database mydb to myuser;
```

```
psql -h 127.0.0.1 -p 5432 -U myuser mydb
```

### Install TimescaleDB

Ref https://docs.timescale.com/latest/getting-started/installation/ubuntu/installation-apt-ubuntu

```
sudo apt install -y software-properties-common
sudo add-apt-repository -y ppa:timescale/timescaledb-ppa
sudo apt update
```

```
sudo apt install -y timescaledb-2-postgresql-12
```

```
sudo timescaledb-tune -pg-version 12
# or
sudo vim /etc/postgresql/12/main/postgresql.conf
shared_preload_libraries = 'timescaledb'

sudo systemctl restart postgresql@12-main
```

```
sudo su - postgres
psql -p 5433

postgres=# create database mydb_timescale;
postgres=# create user myuser with encrypted password 'mypass';
postgres=# grant all privileges on database mydb_timescale to myuser;
postgres=# \c mydb_timescale
mydb_timescale=# CREATE EXTENSION IF NOT EXISTS timescaledb;
```

```
psql -h 127.0.0.1 -p 5433 -U myuser mydb_timescale

mydb_timescale=> \dx
```

### Server database dir move

```
sudo systemctl stop postgresql
ps aux | grep -i '[p]ostgres'

sudo mkdir /data/postgresql
sudo rsync -aqxP /var/lib/postgresql/ /data/postgresql

sudo rm -rf /var/lib/postgresql
sudo ln -sf /data/postgresql /var/lib/postgresql

sudo systemctl start postgresql
ps aux | grep -i '[p]ostgres'
```
