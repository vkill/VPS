## Redis

### Install Common

Ref https://redis.io/download#from-the-official-ubuntu-ppa

```
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

### Server database dir move

```
sudo systemctl stop redis-server
ps aux | grep -i '[r]edis-server'

sudo mkdir /data/redis
sudo rsync -aqxP /var/lib/redis/ /data/redis

sudo rm -rf /var/lib/redis
sudo ln -sf /data/redis /var/lib/redis

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

