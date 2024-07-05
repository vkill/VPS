## Rust

### Install

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

# Or

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs -o /tmp/rustup-init.sh
sh /tmp/rustup-init.sh -y -v --default-toolchain=1.75.0
```

```
sudo apt install -y gcc pkg-config libssl-dev

sudo yum install -y gcc openssl-devel
```

```
source $HOME/.cargo/env

vim ~/.zshrc
source $HOME/.cargo/env
```

```
rustup install stable
rustup default stable
```

### Install on Windows

https://www.rust-lang.org/tools/install

Download `rustup-init.exe` and run, select `1) Quick install via the Visual Studio Community installer`

### Configure source (For CN)

```
vim ~/.cargo/config

[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"

replace-with = 'tuna'

[source.tuna]
registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"

[source.ustc]
registry = "git://mirrors.ustc.edu.cn/crates.io-index"

[source.sjtu]
registry = "https://mirrors.sjtug.sjtu.edu.cn/git/crates.io-index"
```

### rdkafka

```
# Ubuntu 22.04
sudo apt install -y librdkafka-dev
```

But rdkafka 0.31 Requested 'rdkafka >= 1.9.2' but version of librdkafka is 1.8.0
