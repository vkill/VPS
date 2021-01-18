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
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https

sudo firewall-cmd --reload
firewall-cmd --zone=public --list-all
```

```
sudo firewall-cmd --permanent --new-zone=pg_xx
sudo firewall-cmd --reload
firewall-cmd --get-zones

sudo firewall-cmd --permanent --zone=pg_xx --add-source=192.168.1.2
sudo firewall-cmd --permanent --zone=pg_xx --add-source=192.168.2.0/24
sudo firewall-cmd --permanent --zone=pg_xx --add-port=5432/tcp

sudo firewall-cmd --reload
firewall-cmd --zone=pg_xx --list-all
```

```
sudo firewall-cmd --permanent --new-ipset=ipset_outgoing_xx --type=hash:net
sudo firewall-cmd --reload
firewall-cmd --get-ipsets

sudo firewall-cmd --permanent --ipset=ipset_outgoing_xx --add-entry=1.1.1.1
sudo firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 1 -m set --match-set ipset_outgoing_xx dst -p tcp -j DROP
sudo firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 2 -j ACCEPT

sudo firewall-cmd --reload
firewall-cmd --direct --get-all-rules

iptables -nvL OUTPUT
```
