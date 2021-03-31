## VPS

### sysbench

```
sudo apt install -y sysbench
```

```
sudo sysbench cpu run
```

### Note

Change password regularly

### Linode (Arch Linux)

```
ssh -A root@x.x.x.x
```

```
# as root

pacman -Syu

useradd -m archlinux

passwd archlinux

echo '%archlinux ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

su - archlinux
```

```
# as archlinux

mkdir -p ~/.ssh
chmod 700 ~/.ssh

touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys

vim ~/.ssh/authorized_keys
```
