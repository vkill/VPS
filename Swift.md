## Swift

### Install

Ref https://gist.github.com/Jswizzy/408af5829970f9eb18f9b45f891910bb

```
sudo apt install clang libpython2.7 libpython2.7-dev

wget https://swift.org/builds/swift-5.7.3-release/ubuntu2204/swift-5.7.3-RELEASE/swift-5.7.3-RELEASE-ubuntu22.04.tar.gz

tar -zxvf swift-5.7.3-RELEASE-ubuntu22.04.tar.gz

sudo mv swift-5.7.3-RELEASE-ubuntu22.04 /opt/swift-5.7.3

echo "export PATH=/opt/swift-5.7.3/usr/bin:$PATH" >> ~/.zshrc
source ~/.zshrc

swift --version
```

```
sudo apt install -y pkg-config libssl-dev
```
