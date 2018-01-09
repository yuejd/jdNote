### compile ###
```
sudo apt-get install python-dev
sudo apt-get install python3-dev
sudo apt-get install libncurses5-dev libgnome2-dev libgnomeui-dev libgtk2.0-dev libatk1.0-dev libbonoboui2-dev libcairo2-dev libx11-dev libxpm-dev libxt-dev
./configure --with-features=huge --enable-gui=auto --enable-gtk2-check --enable-gnome-check --with-x --enable-python3interp --enable-pythoninterp --with-python-config-dir=/usr/lib/python2.7/config-x86_64-linux-gnu/ --enable-rubyinterp --enable-luainterp --enable-perlinterp --with-python3-config-dir=/usr/lib/python3.5/config-3.5m-x86_64-linux-gnu/ --enable-multibyte --enable-cscope --enable-gui=gnome2 --prefix=/usr/local --with-compiledby="jdyue19@gmail.com"
make
sudo checkinstall
dpkg -l | grep vim
vim --version
sudo apt-mark hold vim # unhold to release
```

### configure ###
```
git clone https://github.com/yuejd/k-vim.git
git branch -a
git checkout -b xxx origin/jd171231
```
#### dependency ####
```
sudo pip install flake8 yapf

# ubuntu
sudo apt-get install ctags
sudo apt-get install build-essential cmake python-dev
sudo apt-get install silversearcher-ag

# centos
sudo yum install python-devel.x86_64
sudo yum groupinstall 'Development Tools'
sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo yum install the_silver_searcher
sudo yum install cmake

# mac
brew install ctags
brew install the_silver_searcher
```

##### install #####
```
cd k-vim
./install.sh --for-vim
```

##### uninstall #####
```
cd ~ && rm -rf .vim .vimrc .vimrc.bundles && cd -
```
