## System IO Tools

### IO Read / IO Write

```
sudo yum install -y hdparm

sudo su - root
# read
hdparm -t --direct /dev/sda2
# write
sync;time -p bash -c "(dd if=/dev/zero of=/test.dd  bs=1M count=20480;sync)"
```

```
iostat -x -m 2
```
