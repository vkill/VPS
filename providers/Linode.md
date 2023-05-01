## Linode

### Account Settings

Enable "Network Helper", verify via `sudo cat /etc/systemd/network/05-eth0.network`

### IPv4 – Private not showing/working

```
$ sudo vim /etc/systemd/network/05-eth0.network

[Match]
Name=eth0

[Network]
DHCP=ipv4
Domains=members.linode.com
IPv6PrivacyExtensions=false
# Add it manually
Address=192.168.x.x/17

$ sudo systemctl restart systemd-networkd
```
