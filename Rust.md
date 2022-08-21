## Rust

### Install

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

```
sudo apt install -y gcc pkg-config libssl-dev

sudo yum install -y openssl-devel
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

### Configure source (For CN)

```
vim ~/.cargo/config

[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"

replace-with = 'rustcc'

[source.tuna]
registry = "https://mirrors.tuna.tsinghua.edu.cn/git/crates.io-index.git"

[source.ustc]
registry = "git://mirrors.ustc.edu.cn/crates.io-index"

[source.sjtu]
registry = "https://mirrors.sjtug.sjtu.edu.cn/git/crates.io-index"

[source.rustcc]
registry = "git://crates.rustcc.cn/crates.io-index"
```
