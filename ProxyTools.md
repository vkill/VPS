## Proxy Tools

### Shadowsocks-libev (For CN)

```
sudo apt install -y shadowsocks-libev
```

```
cd /etc/yum.repos.d
curl -O https://copr.fedorainfracloud.org/coprs/librehat/shadowsocks/repo/epel-7/librehat-shadowsocks-epel-7.repo
yum -y install shadowsocks-libev
```

```
sudo vim /etc/shadowsocks-libev/local1.json
{
    "server":"jp2-sta97.godie.space",
    "server_port":27251,
    "local_port":1080,
    "password":"jYaUHrstyPY3LYa",
    "timeout":60,
    "method":"chacha20-ietf-poly1305"
}

sudo systemctl enable shadowsocks-libev-local@local1.service
sudo systemctl restart shadowsocks-libev-local@local1.service

sudo systemctl stop shadowsocks-libev.service
sudo systemctl disable shadowsocks-libev.service

curl -x socks5://127.0.0.1:1080 https://www.baidu.com -v -I
```

### ProxyChains (For CN)

```
sudo apt install -y proxychains4
```

```
sudo vim /etc/proxychains4.conf
socks5 127.0.0.1 1080
```

### Configure git (For CN)

```
git config --global http.proxy socks5://127.0.0.1:1080
git config --global https.proxy socks5://127.0.0.1:1080
```
