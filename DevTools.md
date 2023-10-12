## Dev Tools

### Oh My Zsh

https://github.com/robbyrussell/oh-my-zsh

```
sudo apt install -y zsh git
sudo yum install -y zsh git

# Maybe require `sudo dnf install -y util-linux-user`
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
