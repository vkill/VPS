## TimescaleDB

Require install PostgreSQL 13 server and client

### Install

Ref https://docs.timescale.com/latest/getting-started/installation/ubuntu/installation-apt-ubuntu

```
sudo apt install -y software-properties-common
sudo add-apt-repository -y ppa:timescale/timescaledb-ppa
sudo apt update
```

```
sudo apt install -y timescaledb-2-postgresql-13
```

```
sudo timescaledb-tune -pg-version 13
# or
sudo vim /etc/postgresql/13/main/postgresql.conf
shared_preload_libraries = 'timescaledb'

sudo systemctl restart postgresql@13-main
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
