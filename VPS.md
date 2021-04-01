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

useradd -m arch

passwd arch

echo '%arch ALL=(ALL) NOPASSWD: ALL' >> /etc/sudoers

su - arch
```

```
# as arch

mkdir -p ~/.ssh
chmod 700 ~/.ssh

touch ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys

vim ~/.ssh/authorized_keys
```
