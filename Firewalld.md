## Firewalld

### Install

```
sudo apt install -y firewalld
```

```
sudo systemctl start firewalld
sudo systemctl enable firewalld
```

### Configure

```
firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=https
```

```
firewall-cmd --reload

firewall-cmd --permanent --list-all

firewall-cmd --direct --get-all-rules
```
