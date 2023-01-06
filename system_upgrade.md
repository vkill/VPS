## System Upgrade

### Ubuntu

ref https://www.linuxtechi.com/upgrade-ubuntu-18-04-lts-to-ubuntu-20-04-lts/

```
cat /etc/lsb-release

sudo apt update
sudo apt upgrade -y
sudo apt dist-upgrade
sync && sync && sudo shutdown -r now
```

```
sudo apt --purge autoremove -y
sudo apt install update-manager-core -y

# 18.04 -> 20.04
# 20.04 -> 22.04
sudo do-release-upgrade -d

# 16.04 -> 18.04
sudo do-release-upgrade
```

### Ubuntu 20.04 kernel x.x.x-x to 5.10.4-051004

ref https://sypalo.com/how-to-upgrade-ubuntu

```
cd /tmp

wget -c https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.10.4/amd64/linux-headers-5.10.4-051004_5.10.4-051004.202012301142_all.deb
wget -c https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.10.4/amd64/linux-headers-5.10.4-051004-generic_5.10.4-051004.202012301142_amd64.deb
wget -c https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.10.4/amd64/linux-image-unsigned-5.10.4-051004-generic_5.10.4-051004.202012301142_amd64.deb
wget -c https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.10.4/amd64/linux-modules-5.10.4-051004-generic_5.10.4-051004.202012301142_amd64.deb

sudo dpkg -i *.deb

dpkg --list 'linux-image-*'

sudo apt purge linux-image-x.x.x-x-generic
sudo apt purge linux-image-unsigned-x.x.x-x-generic

sudo update-grub

sync && sync && sudo shutdown -r now
```

```
uname -r
```

### Ubuntu 20.04 kernel 5.10.4-051004 to default

```
sudo apt install linux-generic

dpkg --list 'linux-image-*'

sudo apt purge linux-image-5.10.4-051004-generic
sudo apt purge linux-image-unsigned-5.10.4-051004-generic

sudo update-grub

sync && sync && sudo shutdown -r now
```

```
uname -r
```
