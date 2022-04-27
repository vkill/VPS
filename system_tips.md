## System Tips

### Clear atop log files

```
find /var/log/atop/ -name 'atop_*' -mtime +7 | xargs -I{} rm {}
```
