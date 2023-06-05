## Protocol Buffers

### Install

```
# Ref https://git.launchpad.net/ubuntu/+source/protobuf/
sudo apt install -y protobuf-compiler
```

```
# AlmaLinux
sudo dnf --enablerepo=crb install -y protobuf-compiler
```

### Binary

```
sudo yum install -y unzip

cd /usr/local/src
sudo wget https://github.com/protocolbuffers/protobuf/releases/download/v23.1/protoc-23.1-linux-x86_64.zip
sudo unzip protoc-23.1-linux-x86_64.zip
sudo mv bin/* /usr/local/bin/
sudo mv include/* /usr/local/include/
protoc --version
```

### Tips

Do not use Protobuf 3 in CentOS 7
