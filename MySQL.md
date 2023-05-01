## MySQL

### Install Client

```
sudo apt install -y mysql-client-8.0
```

### Install Server

```
sudo apt install -y mysql-server-8.0

sudo systemctl enable mysql
sudo systemctl restart mysql
```

```
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
bind-address = 127.0.0.1,192.168.1.2
```

```
mysql
> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'xxx';

> CREATE DATABASE mydb;
> CREATE USER 'myuser'@'%' IDENTIFIED BY 'mypass';
> GRANT ALL PRIVILEGES ON mydb.* TO 'myuser'@'%';
> FLUSH PRIVILEGES;

> exit

mysql -h 127.0.0.1 -P 3306 -u myuser -p mydb
```
