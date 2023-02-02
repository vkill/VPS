## System Dev

### Package content

```
sudo apt install -y apt-file
sudo apt-file update
apt-file list curl

sudo yum install -y yum-utils
repoquery -l curl
```

### ripgrep

```
sudo apt install -y ripgrep
```

### tree

```
sudo apt install -y tree
```

### tig

```
sudo apt install -y tig
sudo yum install -y tig
```

### Network bandwidth

```
sudo apt install -y nload iftop

sudo nload eth0
sudo iftop -i eth0 -P
```
