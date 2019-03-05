# git

### --bare with dotfiles
```ruby
git init --bare $HOME/.dotfiles
echo "alias config='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME'" >> $HOME/.zshrc
source $HOME/.zshrc
config config --local status.showUntrackedFiles no

config status
config add ~/.zshrc
config commit -m "Add vimrc"

config remote add origin git@gitlab.com:yourname/dotfiles.git
config push -u origin master

# setting up new machines
git clone --bare git@gitlab.com:yourname/dotfiles.git $HOME/.cfg

# from here, you can pull, push, merge and checkout to your hearts content.
```