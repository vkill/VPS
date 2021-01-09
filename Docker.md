## Docker

### Docker Install

https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-repository

```
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt update

sudo apt-get install -y docker-ce docker-ce-cli

sudo usermod -a -G docker $USER

sudo systemctl restart docker
```

### DockerComposer Install

https://github.com/docker/compose/releases

```
sudo curl -L https://github.com/docker/compose/releases/download/1.27.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

### Docker dir move

https://linuxconfig.org/how-to-move-docker-s-default-var-lib-docker-to-another-directory-on-ubuntu-debian-linux

```
sudo systemctl stop docker
ps aux | grep -i '[d]ocker'

sudo sed -i 's!ExecStart=/usr/bin/dockerd -H fd://!ExecStart=/usr/bin/dockerd -g /data/docker -H fd://!' /lib/systemd/system/docker.service

sudo mkdir /data/docker
sudo rsync -aqxP /var/lib/docker/ /data/docker

sudo systemctl daemon-reload
sudo systemctl start docker
ps aux | grep -i '[d]ocker'

sudo rm -rf /var/lib/docker/
```
