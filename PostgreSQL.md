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
