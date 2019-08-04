---
id: mac-setup
title: Mac setup
---

# Mac setup apps

Run script this script to install apps with brew.

First create the install file. Use the content below and save it as `install.sh`.

Give the file execution permissions and run it:

```
chmod +x /path/to/install.sh

# Run the script
/path/to/yourscript.sh
```


## install.sh

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

brew cask install google-chrome
brew cask install spotify

brew cask install spectacle
brew install node
brew instal npm

brew cask install firefox
brew cask install visual-studio-code

brew cask install fork
brew cask install sequel-pro
brew cask install graphiql
brew cask install vlc
brew cask install postman
brew cask install kap
brew install duck


brew cask install sketch
brew cask install virtualbox
brew install docker docker-machine

brew install tree

echo "alias sshkey='pbcopy < ~/.ssh/id_rsa.pub'" >> ~/.bash_profile
```