## Dev Tools

### Oh My Zsh

https://github.com/robbyrussell/oh-my-zsh

```
sudo apt install -y zsh git
sudo yum install -y zsh git

sudo chsh -s /bin/zsh $USER

curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```

### tmux

https://github.com/tmux/tmux

```
sudo apt install -y tmux
sudo yum install -y tmux

cd ~
git clone https://github.com/gpakosz/.tmux.git
ln -s -f .tmux/.tmux.conf
cp .tmux/.tmux.conf.local .
```

### Other

```
sudo apt install -y tig
sudo yum install -y tig
```

```
sudo apt install -y ripgrep
```

```
sudo apt install -y tree
```
