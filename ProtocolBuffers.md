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

### Tips

Do not use Protobuf 3 in CentOS 7
