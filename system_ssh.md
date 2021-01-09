## System SSH

### Init

```
mkdir -p ~/.ssh \
  && chmod 700 ~/.ssh \
  && touch ~/.ssh/id_rsa.pub \
  && chmod 600 ~/.ssh/id_rsa.pub \
  && touch ~/.ssh/authorized_keys \
  && chmod 600 ~/.ssh/authorized_keys \
  && echo YOUR_SSH_PUBLIC_KEY > ~/.ssh/id_rsa.pub \
  && cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

### SSH Sockets

```
$ mkdir ~/.ssh/sockets
$ chmod 700 ~/.ssh/sockets
$ touch ~/.ssh/config
$ chmod 600 ~/.ssh/config
$ vim ~/.ssh/config

Host *
  ControlPersist 24h
  ControlMaster auto
  ControlPath ~/.ssh/sockets/%r@%h-%p
  Compression yes
  ServerAliveInterval 60
  ServerAliveCountMax 5
```
