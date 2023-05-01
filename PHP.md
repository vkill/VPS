## PHP

```
sudo apt update
sudo apt install software-properties-common
sudo apt-add-repository --yes --update ppa:ondrej/php

sudo apt install -y php7.4 php7.4-fpm
sudo apt install -y php7.4-mysql php7.4-gd php7.4-curl php7.4-zip php7.4-xml

sudo vim /etc/php/7.4/fpm/pool.d/www.conf
listen = '127.0.0.1:9000'

sudo systemctl enable php7.4-fpm
sudo systemctl restart php7.4-fpm
```
