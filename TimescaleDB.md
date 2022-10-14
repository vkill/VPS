## TimescaleDB

### Install common

Ref https://docs.timescale.com/latest/getting-started/installation/ubuntu/installation-apt-ubuntu

```
sudo sh -c "echo 'deb https://packagecloud.io/timescale/timescaledb/ubuntu/ `lsb_release -c -s` main' > /etc/apt/sources.list.d/timescaledb.list"

wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | sudo apt-key add -
# or
wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | sudo sh -c "gpg --dearmor > /etc/apt/trusted.gpg.d/timescaledb.gpg"

sudo apt update
```

### Install with PostgreSQL 14

```
sudo apt install -y timescaledb-2-postgresql-14
```

```
sudo timescaledb-tune -pg-version 14
# or
sudo vim /etc/postgresql/14/main/postgresql.conf
shared_preload_libraries = 'timescaledb'

sudo systemctl restart postgresql@14-main
```

```
sudo su - postgres
psql -p 5432

postgres=# create database mydb_timescale;
postgres=# create user myuser with encrypted password 'mypass';
postgres=# grant all privileges on database mydb_timescale to myuser;
postgres=# \c mydb_timescale
mydb_timescale=# CREATE EXTENSION IF NOT EXISTS timescaledb;
```

```
psql -h 127.0.0.1 -p 5432 -U myuser mydb_timescale

mydb_timescale=> \dx
```
