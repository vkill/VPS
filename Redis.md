## Redis

### Install Common

Ref https://redis.io/download#from-the-official-ubuntu-ppa

```
sudo apt install -y software-properties-common
sudo add-apt-repository -y ppa:redislabs/redis
sudo apt update
```

### Install Client

```
sudo apt install -y redis-tools
```

### Install Server

```
sudo apt install -y redis-server

sudo systemctl start redis-server
sudo systemctl status redis-server
sudo systemctl enable redis-server
```

```
sudo vim /etc/redis/redis.conf
aclfile /etc/redis/users.acl

echo -n 'mypass' | sha256sum
ea71c25a7a602246b4c39824b855678894a96f43bb9b71319c39700a1e045222  -

sudo vim /etc/redis/users.acl
user default on #ea71c25a7a602246b4c39824b855678894a96f43bb9b71319c39700a1e045222 ~* +@all

sudo systemctl restart redis-server
```

```
redis-cli -h 127.0.0.1 -p 6379 -n 0
```

### Server multiple instances

```
sudo cp /lib/systemd/system/redis-server.service /lib/systemd/system/redis-server@.service

sudo sed -i 's!/etc/redis!/etc/redis-%i!; s!/var/lib/redis!/var/lib/redis-%i!; s!/var/log/redis!/var/log/redis-%i!; s!/var/run/redis!/var/run/redis-%i!; s!^RuntimeDirectory=redis!RuntimeDirectory=redis-%i!' /lib/systemd/system/redis-server@.service
```

```
instance_name=second
instance_port=6380

sudo cp -rp /etc/redis "/etc/redis-${instance_name}"
sudo sed -i "s@^port 6379@port ${instance_port}@; s@/etc/redis@/etc/redis-${instance_name}@; s@/var/lib/redis@/var/lib/redis-${instance_name}@; s@/var/log/redis@/var/log/redis-${instance_name}@; s@/var/run/redis@/var/run/redis-${instance_name}@" "/etc/redis-${instance_name}/redis.conf"

sudo mkdir "/var/log/redis-${instance_name}"
sudo chown redis:adm "/var/log/redis-${instance_name}"
sudo chmod 750 "/var/log/redis-${instance_name}"
sudo chmod g+s "/var/log/redis-${instance_name}"

sudo mkdir "/var/lib/redis-${instance_name}"
sudo chown redis:redis "/var/lib/redis-${instance_name}"
sudo chmod 750 "/var/lib/redis-${instance_name}"

sudo cp /etc/logrotate.d/redis-server "/etc/logrotate.d/redis-server-${instance_name}"
sudo sed -i "s@/var/log/redis@/var/log/redis-${instance_name}@" "/etc/logrotate.d/redis-server-${instance_name}"
```

```
sudo systemctl enable redis-server@second
```

### Server database dir move

```
sudo systemctl stop redis-server
ps aux | grep -i '[r]edis-server'

sudo mkdir /media/data1/redis
sudo rsync -aqxP /var/lib/redis/ /media/data1/redis

sudo rm -rf /var/lib/redis
sudo ln -sf /media/data1/redis /var/lib/redis

sudo systemctl start redis-server
ps aux | grep -i '[r]edis-server'
```

### Tips for /etc/redis/redis.conf

```
bind 127.0.0.1 ::1 192.168.136.55
```

```
# # 30GB == 30 * 1024 * 1024 * 1024 == 32212254720
# # config set maxmemory 32212254720
maxmemory 32212254720
```

```
io-threads 1
```

```
The error "OOM command not allowed when used memory > 'maxmemory'"
# config set maxmemory-policy volatile-ttl
maxmemory-policy volatile-ttl
```

