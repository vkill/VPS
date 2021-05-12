## System Journald

### Size limit

```
sudo vim /etc/systemd/journald.conf
[Journal]
SystemMaxUse=2G
```

### Clean

```
sudo journalctl --vacuum-time=2d

sudo journalctl --vacuum-size=500M
```
