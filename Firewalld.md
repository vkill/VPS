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

firewall-cmd --reload
firewall-cmd --zone=public --list-all
```

```
firewall-cmd --permanent --new-zone=pg_xx
firewall-cmd --reload
firewall-cmd --get-zones

firewall-cmd --permanent --zone=pg_xx --add-source=x.x.x.x
firewall-cmd --permanent --zone=pg_xx --add-source=y.y.y.y
firewall-cmd --permanent --zone=pg_xx --add-port=5432/tcp

firewall-cmd --reload
firewall-cmd --zone=pg --list-all
```

```
firewall-cmd --permanent --new-ipset=ipset_outgoing_xx --type=hash:net
firewall-cmd --reload
firewall-cmd --get-ipsets

firewall-cmd --permanent --ipset=ipset_outgoing_xx --add-entry=x.x.x.x
firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 1 -m set --match-set ipset_outgoing_xx dst -p tcp -j DROP
firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 2 -j ACCEPT

firewall-cmd --reload
firewall-cmd --direct --get-all-rules
```
