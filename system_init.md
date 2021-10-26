## System Init

### Change Password

```
passwd
```

### /etc/sudoers

```
# e.g. wholesaleinternet
su
echo '%customer ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers
```

### Upgrade && Update

```
sudo apt update
sudo apt upgrade -y
sudo apt --purge autoremove -y

sudo yum update -y
sudo yum install -y yum-utils
sudo yum remove -y `package-cleanup --leaves`
```

### Packages

```
sudo apt install -y vim htop net-tools sysstat git curl wget nload
sudo apt install -y gnupg2 iputils-ping

sudo yum install -y epel-release
sudo yum install -y vim htop wget net-tools sysstat git curl wget
```

### Mount /media/sdb

```
sudo apt install -y xfsprogs
```

```
sudo mkfs.xfs /dev/sdb
# sudo xfs_growfs /dev/sdb

sudo mkdir /media/sdb
sudo mount /dev/sdb /media/sdb

sudo ls -l /dev/disk/by-uuid/

sudo vim /etc/fstab
UUID=xxxxxx /media/sdb auto defaults 0 0
```

### Dirs

```
sudo mkdir /media/sdb/data
sudo ln -sf /media/sdb/data /
#
# OR
#
sudo mkdir /data
```

```
sudo mkdir /media/sdb/repos
sudo ln -sf /media/sdb/repos /
#
# OR
#
sudo mkdir /repos

sudo chown $USER /repos
```

### Time zone

```
sudo timedatectl set-timezone UTC
```

### NTP

```
sudo apt install -y ntp
```

```
sudo systemctl enable ntp
```

### Hostname

```
sudo hostnamectl set-hostname xxx

sudo vim /etc/hosts
127.0.0.1 xxx
```

### limits

```
sudo vim /etc/security/limits.d/91-nofiles.conf

* soft noproc 51200
* hard noproc 51200

* soft nofile 51200
* hard nofile 51200

root soft noproc 51200
root hard noproc 51200

root soft nofile 51200
root hard nofile 51200
```

### BBR

```
sudo vim /etc/sysctl.d/91-bbr.conf
net.core.default_qdisc=fq
net.ipv4.tcp_congestion_control=bbr

sudo sysctl -p /etc/sysctl.d/91-bbr.conf

sysctl net.ipv4.tcp_congestion_control
```

### SSHD Port (optional)

```
sudo vim /etc/ssh/sshd_config
Port 1022
```

### Add Swap (optional)

Ref https://wiki.archlinux.org/index.php/swap

```
# General
sudo fallocate -l 4G /swapfile
# Centos 7
sudo dd if=/dev/zero of=/swapfile count=4096 bs=1MiB

sudo chmod 600 /swapfile

sudo mkswap /swapfile

sudo swapon /swapfile

sudo vim /etc/fstab
/swapfile none swap defaults 0 0
```
