## Nginx

### Install

https://www.nginx.com/resources/wiki/start/topics/tutorials/install/

```
sudo sh -c 'echo "deb https://nginx.org/packages/ubuntu/ $(lsb_release -cs) nginx" > /etc/apt/sources.list.d/nginx.list'
sudo sh -c 'echo "deb-src https://nginx.org/packages/ubuntu/ $(lsb_release -cs) nginx" >> /etc/apt/sources.list.d/nginx.list'

sudo apt update
# if got "GPG error: ... public key is not available: NO_PUBKEY ABF5BD827BD9BF62"
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys ABF5BD827BD9BF62
sudo apt update

sudo apt install -y nginx
# Or 
# sudo apt install -y nginx-full
sudo apt install -y nginx-module-njs nginx-module-njs-dbg
```

```
sudo vim /etc/yum.repos.d/nginx.repo
[nginx]
name=nginx repo
baseurl=https://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=0
enabled=1

sudo yum install -y nginx
sudo yum install -y nginx-module-njs nginx-module-njs-debuginfo
```


```
sudo systemctl enable nginx

sudo systemctl start nginx
```

### Configure

```
sudo mkdir -p /etc/nginx/http_servers

sudo mkdir -p /etc/nginx/stream_servers

sudo mkdir -p /etc/nginx/certs

sudo mv /etc/nginx/nginx.conf /etc/nginx/nginx.conf.OG

sudo vim /etc/nginx/nginx.conf

load_module modules/ngx_http_js_module.so;
# load_module modules/ngx_http_js_module-debug.so;
load_module modules/ngx_stream_js_module.so;
# load_module modules/ngx_stream_js_module-debug.so;

user nginx;
worker_processes 1;
events {
    worker_connections 10240;
}

error_log /var/log/nginx/error.log warn;
pid /var/run/nginx.pid;

http {
    include mime.types;
    default_type application/octet-stream;
    sendfile on;
    keepalive_timeout 65;

    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

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
vim /etc/nginx/http_servers/xxx.conf
```

```
sudo nginx -t
sudo systemctl restart nginx
```

```
sudo mkdir -p /media/data1/nginx_cache
sudo chown nginx /media/data1/nginx_cache
sudo chmod 700 /media/data1/nginx_cache
```

### fastcgi.conf

```
$ sudo vim /etc/nginx/fastcgi.conf

fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
fastcgi_param  QUERY_STRING       $query_string;
fastcgi_param  REQUEST_METHOD     $request_method;
fastcgi_param  CONTENT_TYPE       $content_type;
fastcgi_param  CONTENT_LENGTH     $content_length;

fastcgi_param  SCRIPT_NAME        $fastcgi_script_name;
fastcgi_param  REQUEST_URI        $request_uri;
fastcgi_param  DOCUMENT_URI       $document_uri;
fastcgi_param  DOCUMENT_ROOT      $document_root;
fastcgi_param  SERVER_PROTOCOL    $server_protocol;
fastcgi_param  REQUEST_SCHEME     $scheme;
fastcgi_param  HTTPS              $https if_not_empty;

fastcgi_param  GATEWAY_INTERFACE  CGI/1.1;
fastcgi_param  SERVER_SOFTWARE    nginx/$nginx_version;

fastcgi_param  REMOTE_ADDR        $remote_addr;
fastcgi_param  REMOTE_PORT        $remote_port;
fastcgi_param  REMOTE_USER        $remote_user;
fastcgi_param  SERVER_ADDR        $server_addr;
fastcgi_param  SERVER_PORT        $server_port;
fastcgi_param  SERVER_NAME        $server_name;

# PHP only, required if PHP was built with --enable-force-cgi-redirect
fastcgi_param  REDIRECT_STATUS    200;

```

### Tips

```
# cat /var/log/nginx/error.log
# 10240 worker_connections exceed open file resource limit: 1024

sudo vim /etc/systemd/system/multi-user.target.wants/nginx.service
[Service]
LimitNOFILE=20480
LimitNPROC=20480

# OR

sudo mkdir /etc/systemd/system/nginx.service.d
sudo vim /etc/systemd/system/nginx.service.d/limit.conf
[Service]
LimitNOFILE=20480
LimitNPROC=20480
```

```
# CentOS 7
# systemctl status nginx.service
# nginx: [emerg] bind() to 0.0.0.0:8091 failed (13: Permission denied)
# ref https://www.itadminstrator.com/2017/07/nginx-no-permission-to-bind-port-9080.html

sudo yum install policycoreutils policycoreutils-python -y
sudo semanage port -l | grep ^"http_port_t"

sudo semanage port -a -t http_port_t -p tcp 8091
# or
# if got "ValueError: Port tcp/8091 already defined"
sudo semanage port -m -t http_port_t -p tcp 8091

sudo semanage port -l | grep ^"http_port_t"
```

```
# CentOS 7
# bind() to unix:/var/run/nginx_xxx.sock failed (98: Address already in use)
# "/var/run/nginx.pid" failed (13: Permission denied)

sudo vim /etc/systemd/system/multi-user.target.wants/nginx.service
[Service]
# PIDFile=/var/run/nginx.pid
# ExecReload=/bin/sh -c "/bin/kill -s HUP $(/bin/cat /var/run/nginx.pid)"
# ExecStop=/bin/sh -c "/bin/kill -s TERM $(/bin/cat /var/run/nginx.pid)"
ExecReload=/bin/sh -c "/bin/kill -s HUP $MAINPID"
ExecStop=/bin/sh -c "/bin/kill -s TERM $MAINPID"
```

```
# systemctl restart nginx.service
# if got "start request repeated too quickly for nginx.service"
# ref https://serverfault.com/questions/845471/service-start-request-repeated-too-quickly-refusing-to-start-limit

systemctl reset-failed nginx.service
systemctl start nginx.service
```
