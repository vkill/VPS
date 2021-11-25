## Citus

### Install

Ref https://github.com/citusdata/citus#install-citus-locally

```
curl https://install.citusdata.com/community/deb.sh | sudo bash
sudo apt -y install postgresql-14-citus-10.2
```

```
sudo vim /etc/postgresql/14/main/postgresql.conf
shared_preload_libraries = 'citus'

sudo systemctl restart postgresql@14-main
```

```
sudo su - postgres
psql -p 5432

postgres=# create database mydb_citus;
postgres=# create user myuser with encrypted password 'mypass';
postgres=# grant all privileges on database mydb_citus to myuser;
postgres=# \c mydb_citus
mydb_citus=# CREATE EXTENSION IF NOT EXISTS citus;
```

```
psql -h 127.0.0.1 -p 5432 -U myuser mydb_citus

mydb_citus=> \dx
```
