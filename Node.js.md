## Node.js

### Install via nvm

Ref https://github.com/nvm-sh/nvm#installing-and-updating

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash

vim ~/.zshrc

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

```
nvm install node

nvm use node
```

```
node -v

npm install -g yarn
```

### Install

Ref https://www.ubuntuupdates.org/ppa/node_16.x

Ref https://yarnpkg.com/getting-started/install

### Install via nodeenv

```
sudo pip3 install nodeenv

nodeenv --list
```

```
cd ~

nodeenv node-15.4.0
. node-15.4.0/bin/activate
(node-15.4.0) npm install -g yarn
```
