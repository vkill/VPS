## Rust

### Install

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
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
