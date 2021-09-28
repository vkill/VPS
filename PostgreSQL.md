## PostgreSQL

### Install Common (Ubuntu)

https://wiki.postgresql.org/wiki/Apt

https://www.postgresql.org/download/linux/ubuntu/

```
sudo apt install -y curl ca-certificates gnupg

curl https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

sudo apt update
```

### Install Client (Ubuntu)

```
sudo apt install -y postgresql-client-13
```

### Install Server (Ubuntu)

```
sudo apt install -y postgresql-13

sudo vim /etc/postgresql/13/main/conf.d/listen_addresses.conf
listen_addresses = '0.0.0.0'

sudo vim /etc/postgresql/13/main/pg_hba.conf
host all all 0.0.0.0/0 md5

sudo vim /etc/postgresql/13/main/conf.d/port.conf
port = 5432

sudo vim /etc/postgresql/13/main/conf.d/max_connections.conf
max_connections = 2000

sudo systemctl restart postgresql@13-main
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

### Server multiple instances (Ubuntu)

Ref https://www.percona.com/blog/2019/06/24/managing-multiple-postgresql-instances-on-ubuntu-debian/

```
pg_lsclusters
```

```
sudo pg_createcluster 13 xxx -d /var/lib/postgresql/13/xxx -p 15432

sudo systemctl start postgresql@13-xxx.service
```

```
sudo pg_dropcluster 13 xxx --stop
```

### Server database dir move (Ubuntu)

```
sudo systemctl stop postgresql@13-main
ps aux | grep -i '[p]ostgres'

sudo mkdir /data/postgresql
sudo rsync -aqxP /var/lib/postgresql/ /data/postgresql

sudo rm -rf /var/lib/postgresql
sudo ln -sf /data/postgresql /var/lib/postgresql

sudo systemctl start postgresql@13-main
ps aux | grep -i '[p]ostgres'
```

### Install (CentOS 7)

https://www.postgresql.org/download/linux/redhat/

```
sudo yum install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm

sudo yum install -y postgresql13-server
sudo yum install -y postgresql13-contrib
```

