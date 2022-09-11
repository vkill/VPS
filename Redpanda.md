## Redpanda

### Install

https://docs.redpanda.com/docs/deployment/production-deployment/

```
curl -1sLf 'https://packages.vectorized.io/nzc4ZYQK3WRGd9sy/redpanda/cfg/setup/bash.deb.sh' | sudo -E bash

sudo apt install redpanda -y

sudo systemctl start redpanda
```

```
sudo rpk redpanda mode production

sudo rpk redpanda tune all
```

```
sudo rpk redpanda config bootstrap --id 1 --self 127.0.0.1

sudo systemctl restart redpanda

sudo journalctl -u redpanda -f

sudo netstat -tnulp | grep redpanda
```

### Configure

https://docs.redpanda.com/docs/21.11/cluster-administration/configuration/

```
sudo vim /etc/redpanda/redpanda.yaml

pandaproxy:
    pandaproxy_api:
        - address: "127.0.0.1"
          port: 8082

schema_registry:
    schema_registry_api:
        - address: "127.0.0.1"
          port: 8081
```

## Redpanda Console

### Install

https://docs.redpanda.com/docs/console/installation/

```
sudo apt install redpanda-console -y

sudo systemctl start redpanda-console
```

```
sudo systemctl restart redpanda-console

sudo journalctl -u redpanda-console -f

sudo netstat -tnulp | grep redpanda-c
```

```
open http://localhost:8080
```

### Configure

https://docs.redpanda.com/docs/console/reference/config/

```
sudo vim /etc/redpanda/redpanda-console-config.yaml


```
