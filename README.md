# Ubuntu Setup

## Git setup

git config --global user.email "jerryarn@gmail.com"
git config --global user.name "Jerry Arnold"
git config --global credential.helper store

## zsh

[Install notes](https://phoenixnap.com/kb/install-zsh-ubuntu)

```
sudo apt update
sudo apt install git zsh -y
zsh --version
```

### Initial Configure
```
zsh
```

Press 0 to create emptry .zshrc

### Set zsh as default shell

```
echo $SHELL
/bin/bash

chsh -s $(which zsh)
```

Reboot

### Install Oh my zsh
```
sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

```
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

Edit ~/.zshrc
```
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)

```

## Keys

https://phoenixnap.com/kb/generate-setup-ssh-key-ubuntu

```
mkdir -p $HOME/.ssh
chmod 0700 $HOME/.ssh
ssh-keygen
ssh-copy-id -i [ssh-key-location] [username]@[server-ip-address]
```

## [Starship](https://starship.rs/)

```
curl -sS https://starship.rs/install.sh | sh
```

https://starship.rs/config/

```
ln -s /mnt/d/projects/ubuntu-setup/starship.toml ~/.config/starship.toml
```

## pyenv

```
sudo apt update

sudo apt install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev

curl https://pyenv.run | bash

echo -e 'export PYENV_ROOT="$HOME/.pyenv"\nexport PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc  

echo -e 'eval "$(pyenv init --path)"\neval "$(pyenv init -)"' >> ~/.zshrc

exec "$SHELL"

pyenv install 3.12.3
pyenv global 3.12.3
```

## Docker

```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-compose
```

```
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker

```

```
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

https://philpug.com/2021/11/14/how-to-move-docker-data-directory-to-a-different-mount-ubuntu/

```
sudo service docker stop
```

Add this to /etc/docker/daemon.js:

```
{
   "data-root": "/srv/appdata/docker"
}
```

```
sudo rsync -aP /var/lib/docker/ /new/path/to/your/docker
sudo mv /var/lib/docker /var/lib/docker.old
sudo service docker start
```

confirm
```
docker images
docker ps
```

```
sudo rm -rf /var/lib/docker.old
```
