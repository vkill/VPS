## System Journald

Ref https://wiki.archlinux.org/title/Systemd/Journal

### Size limit

```
sudo vim /etc/systemd/journald.conf
[Journal]
SystemMaxUse=2G

sudo systemctl restart systemd-journald.service
```

### Clean

```
sudo journalctl --vacuum-time=2d

sudo journalctl --vacuum-size=500M
```
