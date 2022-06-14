## Protocol Buffers

### Install

Ref https://git.launchpad.net/ubuntu/+source/protobuf/

```
sudo apt install -y protobuf-compiler
```

```
protoc  | grep _out=OUT_DIR
  --cpp_out=OUT_DIR           Generate C++ header and source.
  --csharp_out=OUT_DIR        Generate C# source file.
  --java_out=OUT_DIR          Generate Java source file.
  --js_out=OUT_DIR            Generate JavaScript source.
  --objc_out=OUT_DIR          Generate Objective C header and source.
  --php_out=OUT_DIR           Generate PHP source file.
  --python_out=OUT_DIR        Generate Python source file.
  --ruby_out=OUT_DIR          Generate Ruby source file.
```

### Build

Ref https://github.com/protocolbuffers/protobuf/blob/main/src/README.md

Ref https://chromium.googlesource.com/chromium/src/third_party/protobuf/

```
sudo apt install -y autoconf automake libtool curl make g++ unzip

git clone https://github.com/protocolbuffers/protobuf.git
cd protobuf
git submodule update --init --recursive
./autogen.sh

./configure
make -j$(nproc)
make check
sudo make install
sudo ldconfig
```

```
protoc  | grep _out=OUT_DIR 
  --cpp_out=OUT_DIR           Generate C++ header and source.
  --csharp_out=OUT_DIR        Generate C# source file.
  --java_out=OUT_DIR          Generate Java source file.
  --kotlin_out=OUT_DIR        Generate Kotlin file.
  --objc_out=OUT_DIR          Generate Objective-C header and source.
  --php_out=OUT_DIR           Generate PHP source file.
  --pyi_out=OUT_DIR           Generate python pyi stub.
  --python_out=OUT_DIR        Generate Python source file.
  --ruby_out=OUT_DIR          Generate Ruby source file.
```
