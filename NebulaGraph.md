## Nebula Graph

### Install

Ref https://github.com/bk-rs/nebula-rs/wiki/Build-Nebula-Graph-1.x-on-Ubuntu-20.04

First of all, [build fbthrift](https://github.com/bk-rs/fbthrift-git-rs/wiki/Build-fbthrift-on-Ubuntu-20.04)

```
sudo apt install -y libreadline-dev
sudo apt install -y doxygen
sudo apt install -y libreadline-dev
```

```
cd ~
git clone https://github.com/vesoft-inc/nebula.git && cd nebula

git checkout -b v1.2.0

sed -i 's/^DEFINE_uint32(max_batch_size, 256,/DEFINE_uint32(max_batch_size, 512,/' src/kvstore/raftex/RaftPart.cpp

git diff

mkdir _build && cd _build
cmake -DENABLE_TESTING=OFF -DCMAKE_BUILD_TYPE=Release ..

make -j $(nproc)
sudo make install

make db_dump
sudo make install db_dump

cd
```

### Configurate

```
sudo apt install -y bc
```

```
sudo mkdir /data/nebula_data

cd /usr/local/nebula

sudo cp etc/nebula-storaged.conf.production etc/nebula-storaged.conf
sudo cp etc/nebula-metad.conf.production etc/nebula-metad.conf
sudo cp etc/nebula-graphd.conf.production etc/nebula-graphd.conf

sudo chmod 644 etc/nebula-storaged.conf
sudo chmod 644 etc/nebula-metad.conf
sudo chmod 644 etc/nebula-graphd.conf

sudo vim etc/nebula-storaged.conf

--data_path=/data/nebula_data
--rocksdb_block_cache=4096


sudo vim etc/nebula-graphd.conf

--enable_authorize=true
```

```
sudo /usr/local/nebula/scripts/nebula.service start all

sudo netstat -tunlp | grep nebula
```

```
sudo /usr/local/nebula/bin/nebula -u root -p nebula --addr=127.0.0.1 --port=3699

> CHANGE PASSWORD root FROM 'nebula' TO 'rootpass';

> CREATE USER myuser WITH PASSWORD 'mypass';

> CREATE SPACE myspace(partition_num=3, replica_factor=1);

> GRANT ROLE DBA ON myspace TO myuser;
```

```
/usr/local/nebula/bin/nebula -u myuser -p mypass --addr=127.0.0.1 --port=3699

> SHOW SPACES;
```

```
cd /data/nebula_data/nebula
/usr/local/nebula/bin/db_dump --space=nba --mode=stat --tags=player
```
