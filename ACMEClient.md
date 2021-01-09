## ACME Client

### acme.sh

https://github.com/Neilpang/acme.sh

```
sudo apt install -y socat git
sudo yum install -y socat git
```

```
sudo mkdir /data/acme_sh
sudo chown $USER /data/acme_sh

cd ~
git clone https://github.com/Neilpang/acme.sh.git
cd acme.sh
./acme.sh --install \
  --home /data/acme_sh \
  --accountemail "xxx@example.com"
cd ~
rm -rf acme.sh
```
