## Citus

### Install common

Ref https://github.com/citusdata/citus#install-citus-locally

```
curl https://install.citusdata.com/community/deb.sh | sudo bash
```

### Install with PostgreSQL 15

```
sudo apt -y install postgresql-15-citus-11.3
```

```
sudo vim /etc/postgresql/15/main/postgresql.conf
shared_preload_libraries = 'citus'

sudo systemctl restart postgresql@15-main
```

```
sudo su - postgres
psql -p 5432

postgres=# create database mydb_citus;
postgres=# \c mydb_citus
mydb_citus=# CREATE EXTENSION IF NOT EXISTS citus;
mydb_citus=> \dx
```
