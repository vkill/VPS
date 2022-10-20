## System Network

### AWS EC2 Multiple IP addresses 

e.g. The Network Interface MAC is 0a:a5:92:c2:c6:d0, Private IPv4 addresses are 172.31.45.1 and 172.31.45.2

```
sudo vim /etc/netplan/50-cloud-init.yaml
network:
    ethernets:
        ens5:
            addresses:
                - 172.31.45.1/20
                - 172.31.45.2/20
            dhcp4: true
            dhcp6: false
            match:
                macaddress: 0a:a5:92:c2:c6:d0
            set-name: ens5
    version: 2

sudo netplan generate

sudo netplan apply
```
