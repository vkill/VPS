## OpenResty

### Install

Ref https://openresty.org/en/linux-packages.html

```
sudo apt-get -y install --no-install-recommends wget gnupg ca-certificates

wget -O - https://openresty.org/package/pubkey.gpg | sudo apt-key add -

echo "deb http://openresty.org/package/ubuntu $(lsb_release -sc) main" \
    | sudo tee /etc/apt/sources.list.d/openresty.list

sudo apt-get update

sudo apt-get -y install --no-install-recommends openresty
```

```
wget https://openresty.org/package/centos/openresty.repo

sudo mv openresty.repo /etc/yum.repos.d/

sudo yum check-update

sudo yum install -y openresty
```

```
sudo systemctl enable openresty

sudo systemctl start openresty
```

### Configure

```
sudo mkdir -p /usr/local/openresty/nginx/conf/http_servers

sudo mkdir -p /usr/local/openresty/nginx/conf/stream_servers

sudo mkdir -p /usr/local/openresty/nginx/conf/certs

sudo mkdir -p /media/data1/openresty/cache
sudo chown nobody /media/data1/openresty/cache
sudo chmod 700 /media/data1/openresty/cache

sudo mv /usr/local/openresty/nginx/conf/nginx.conf /usr/local/openresty/nginx/conf/nginx.conf.OG

sudo vim /usr/local/openresty/nginx/conf/nginx.conf

worker_processes 1;
events {
    worker_connections 10240;
}

http {
    include mime.types;
    default_type application/octet-stream;
    sendfile on;
    keepalive_timeout 65;

    log_format combined_with_request_id '$remote_addr - $remote_user [$time_local] '
                    '$request_time $upstream_response_time '
                    '"$request" $status $body_bytes_sent '
                    '"$http_referer" "$http_user_agent" '
                    '$request_id';

    include http_servers/*.conf;
}

stream {
    log_format basic '$remote_addr [$time_local] '
                    '$protocol $status $bytes_sent $bytes_received '
                    '$session_time';

    include stream_servers/*.conf;
}
```

```
vim /usr/local/openresty/nginx/conf/http_servers/xxx.conf

```

```
sudo /usr/local/openresty/nginx/sbin/nginx -t

sudo systemctl restart openresty
```

### Tips

```
# socket() failed (24: Too many open files)

sudo vim /usr/local/openresty/nginx/conf/nginx.conf

worker_processes 2;
events {
    worker_connections  10240;
}

sudo vim /etc/systemd/system/multi-user.target.wants/openresty.service
[Service]
LimitNOFILE=20480
LimitNPROC=20480

# OR

sudo mkdir /etc/systemd/system/openresty.service.d
sudo vim /etc/systemd/system/openresty.service.d/limit.conf
[Service]
LimitNOFILE=20480
LimitNPROC=20480
```
