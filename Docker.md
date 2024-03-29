## Docker

### Docker Install (Ubuntu)

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

### Docker Install (CentOS)

https://docs.docker.com/engine/install/centos/#install-using-the-repository

```
sudo yum install -y yum-utils

sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

sudo yum install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo usermod -a -G docker $USER

sudo systemctl restart docker
```

### Docker Install (AlmaLinux)

```
sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo

sudo dnf install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo usermod -a -G docker $USER

sudo systemctl restart docker
```

### DockerComposer Install

https://github.com/docker/compose/releases

```
sudo curl -L https://github.com/docker/compose/releases/download/2.17.3/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

### Docker dir move

https://linuxconfig.org/how-to-move-docker-s-default-var-lib-docker-to-another-directory-on-ubuntu-debian-linux

```
sudo systemctl stop docker
ps aux | grep -i '[d]ocker'

sudo mkdir /media/data1/docker
sudo rsync -aqxP /var/lib/docker/ /media/data1/docker

sudo rm -rf /var/lib/docker
sudo ln -sf /media/data1/docker /var/lib/docker

sudo systemctl start docker
ps aux | grep -i '[d]ocker'
```

### Tips

```
sudo docker system df

sudo docker system prune -a

sudo docker system prune -a --volumes
```

```
sudo sh -c 'truncate -s 0 /var/lib/docker/containers/*/*-json.log'

sudo cat /etc/docker/daemon.json
{
    "log-driver": "json-file",
    "log-opts": {"max-size": "10m", "max-file": "3"}
}
```

```
find /var/lib/docker -type f -name '*.log' -size +10M | xargs -I{} truncate -s 0 {}
find /var/lib/docker -type f -name '*.log*.gz' -size +10M | xargs -I{} rm -rf {}
```
