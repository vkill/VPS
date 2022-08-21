## Redis

### Install Common

Ref https://redis.io/download#from-the-official-ubuntu-ppa

```
sudo apt install -y software-properties-common
sudo add-apt-repository -y ppa:redislabs/redis
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
sudo mkdir /etc/redis/modules

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

### Module redis-cell

Ref https://github.com/brandur/redis-cell#install

```
sudo apt install -y gcc pkg-config libssl-dev
```

```
cd /usr/local/src

git clone https://github.com/brandur/redis-cell.git
cd redis-cell
cargo build --release -j 1
sudo cp target/release/libredis_cell.so /etc/redis/modules
```

```
sudo vim /etc/redis/redis.conf
loadmodule /etc/redis/modules/libredis_cell.so
sudo systemctl restart redis-server
```

### Module RedisJSON

Ref https://github.com/RedisJSON/RedisJSON#build

```
sudo apt install -y gcc pkg-config libssl-dev libclang-dev
```

```
cd /usr/local/src

git clone --recursive https://github.com/RedisJSON/RedisJSON.git
cd RedisJSON
cargo build --release -j 1
sudo cp target/release/librejson.so /etc/redis/modules
```

```
sudo vim /etc/redis/redis.conf
loadmodule /etc/redis/modules/librejson.so
sudo systemctl restart redis-server
```

### Module RediSearch

Ref https://redis.io/docs/stack/search/quick_start/#build-from-source

Require python3

```
sudo apt install -y make
```

```
cd /usr/local/src

git clone --recursive https://github.com/RediSearch/RediSearch.git
cd RediSearch
sudo make setup
make build
sudo cp bin/linux-x64-release/search/redisearch.so /etc/redis/modules
```

```
sudo vim /etc/redis/redis.conf
loadmodule /etc/redis/modules/redisearch.so
sudo systemctl restart redis-server
```

### Module RedisTimeSeries

Ref https://redis.io/docs/stack/timeseries/quickstart/#build-and-run-it-yourself

Require python3

```
sudo apt install -y make
```

```
cd /usr/local/src

git clone --recursive https://github.com/RedisTimeSeries/RedisTimeSeries.git
cd RedisTimeSeries
sudo make setup
make build
sudo cp bin/linux-x64-release/redistimeseries.so /etc/redis/modules
```

```
sudo vim /etc/redis/redis.conf
loadmodule /etc/redis/modules/redistimeseries.so
sudo systemctl restart redis-server
```

### Module RedisGears-2.0

Ref https://github.com/RedisGears/RedisGears#build

```
sudo apt install -y gcc pkg-config libssl-dev libclang-dev wget make g++
```

```
cd /usr/local/src

git clone https://github.com/RedisGears/RedisGears.git
cd RedisGears
cargo build --release -j 1
sudo cp target/release/libredisgears.so /etc/redis/modules
sudo cp target/release/libredisgears_v8_plugin.so /etc/redis/modules
```

```
sudo vim /etc/redis/redis.conf
loadmodule /etc/redis/modules/libredisgears.so /etc/redis/modules/libredisgears_v8_plugin.so
sudo systemctl restart redis-server
```

#### redis-server crash when RG.FUNCTION LOAD

```
cat /var/log/syslog

Aug 21 09:46:15 localhost redis-server[10502]: #
Aug 21 09:46:15 localhost redis-server[10502]: # Fatal error in , line 0
Aug 21 09:46:15 localhost redis-server[10502]: # Check failed: 12 == (*__errno_location ()).
Aug 21 09:46:15 localhost redis-server[10502]: #
Aug 21 09:46:15 localhost redis-server[10502]: message repeated 2 times: [ #]
Aug 21 09:46:15 localhost redis-server[10502]: #FailureMessage Object: 0x7ffeee216b90
Aug 21 09:46:15 localhost redis-server[10502]: ==== C stack trace ===============================
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x95b783) [0x7f6a51ca3783]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x1400cb) [0x7f6a514880cb]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x955455) [0x7f6a51c9d455]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x95a824) [0x7f6a51ca2824]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x278af7) [0x7f6a515c0af7]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x27893c) [0x7f6a515c093c]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x278c41) [0x7f6a515c0c41]
Aug 21 09:46:15 localhost kernel: [ 2718.362975] traps: redis-server[10502] trap int3 ip:7f6a51ca2983 sp:7ffeee216b80 error:0 in libredisgears_v8_plugin.so[7f6a51424000+de1000]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x27910d) [0x7f6a515c110d]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x28a77c) [0x7f6a515d277c]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x28b827) [0x7f6a515d3827]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x28b671) [0x7f6a515d3671]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x1f6ad3) [0x7f6a5153ead3]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x1f7a75) [0x7f6a5153fa75]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x515581) [0x7f6a5185d581]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x511d3e) [0x7f6a51859d3e]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x513740) [0x7f6a5185b740]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x51293e) [0x7f6a5185a93e]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x20be98) [0x7f6a51553e98]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x525768) [0x7f6a5186d768]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x1b8540) [0x7f6a51500540]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x1b8ea9) [0x7f6a51500ea9]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x523270) [0x7f6a5186b270]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x15312d) [0x7f6a5149b12d]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x1532bd) [0x7f6a5149b2bd]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x138466) [0x7f6a51480466]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x1036f7) [0x7f6a5144b6f7]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears_v8_plugin.so(+0x101946) [0x7f6a51449946]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears.so(+0xa1746) [0x7f6a5287c746]
Aug 21 09:46:15 localhost redis-server[10502]:     /etc/redis/modules/libredisgears.so(+0xa6d97) [0x7f6a52881d97]
Aug 21 09:46:15 localhost redis-server[10502]:     /usr/bin/redis-server 127.0.0.1:6379(RedisModuleCommandDispatcher+0x4f) [0x5642859e354f]
Aug 21 09:46:15 localhost redis-server[10502]:     /usr/bin/redis-server 127.0.0.1:6379(call+0xee) [0x5642859268ee]
Aug 21 09:46:15 localhost redis-server[10502]:     /usr/bin/redis-server 127.0.0.1:6379(processCommand+0x820) [0x5642859280d0]
Aug 21 09:46:15 localhost redis-server[10502]:     /usr/bin/redis-server 127.0.0.1:6379(processInputBuffer+0x107) [0x564285943237]
Aug 21 09:46:15 localhost redis-server[10502]:     /usr/bin/redis-server 127.0.0.1:6379(readQueryFromClient+0x318) [0x564285943778]
Aug 21 09:46:15 localhost redis-server[10502]:     /usr/bin/redis-server 127.0.0.1:6379(+0x17363c) [0x564285a1a63c]
Aug 21 09:46:15 localhost redis-server[10502]:     /usr/bin/redis-server 127.0.0.1:6379(aeProcessEvents+0x1e2) [0x56428591e2b2]
Aug 21 09:46:15 localhost redis-server[10502]:     /usr/bin/redis-server 127.0.0.1:6379(aeMain+0x1d) [0x56428591e5ed]
Aug 21 09:46:15 localhost redis-server[10502]:     /usr/bin/redis-server 127.0.0.1:6379(main+0x358) [0x564285915d98]
Aug 21 09:46:15 localhost redis-server[10502]:     /lib/x86_64-linux-gnu/libc.so.6(+0x29d90) [0x7f6a53ae2d90]
Aug 21 09:46:15 localhost redis-server[10502]:     /lib/x86_64-linux-gnu/libc.so.6(__libc_start_main+0x80) [0x7f6a53ae2e40]
Aug 21 09:46:15 localhost redis-server[10502]:     /usr/bin/redis-server 127.0.0.1:6379(_start+0x25) [0x564285916425]
Aug 21 09:46:15 localhost systemd[1]: redis-server.service: Main process exited, code=dumped, status=5/TRAP
Aug 21 09:46:15 localhost systemd[1]: redis-server.service: Failed with result 'core-dump'.
Aug 21 09:46:15 localhost systemd[1]: redis-server.service: Scheduled restart job, restart counter is at 1.
Aug 21 09:46:15 localhost systemd[1]: Stopped Advanced key-value store.
```

```
sudo vim /lib/systemd/system/redis-server.service
# change
MemoryDenyWriteExecute=false

sudo systemctl daemon-reload
sudo systemctl restart redis-server
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

