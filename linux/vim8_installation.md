sudo apt-get install python-dev
sudo apt-get install python3-dev
sudo apt-get install libncurses5-dev libgnome2-dev libgnomeui-dev     libgtk2.0-dev libatk1.0-dev libbonoboui2-dev     libcairo2-dev libx11-dev libxpm-dev libxt-dev
./configure --with-features=huge --enable-gui=auto --enable-gtk2-check --enable-gnome-check --with-x --enable-python3interp --enable-pythoninterp --with-python-config-dir=/usr/lib/python2.7/config-x86_64-linux-gnu/ --enable-rubyinterp --enable-luainterp --enable-perlinterp --with-python3-config-dir=/usr/lib/python3.5/config-3.5m-x86_64-linux-gnu/ --enable-multibyte --enable-cscope --enable-gui=gnome2 --prefix=/usr/local --with-compiledby="jdyue19@gmail.com"
make
sudo checkinstall 
dpkg -l | grep vim
vim --version

