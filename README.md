# dotfiles to setup my pentesting environment

## The OS

Start by getting the latest pentesting OS of your choice, currently its still Kali Linux for me, but for you it might be parrot or one of the Arch Linux variants.

At the time of this writing I pulled from OffSec the VMware image of Kali: Kali-Linux-2020.2-vmware-amd64

When starting for the first time, you chose "I copied it" when VMware asks where this image is from. 

!!! I do not recommend using VMWare Workstation Player, as it does not include the snapshot functionality any longer. Either go with Pro, or switch to VirtualBox.

## First steps

#### 1. Update and upgrade the system by running:
```shell
sudo apt update && sudo apt dist-upgrade -y
```

#### 2. (optional) Change your keyboard layout - in my case German - by running:
```shell
sudo dpkg-reconfigure keyboard-configuration
```

Afterwards restart the service so it takes effect:
```shell
sudo service keyboard-setup restart
```

But in many cases a full reboot is required.

#### 3. Change the default password for the "kali" user to something cryptic:
```shell
passwd
```

#### 4. Add a lowpriv daily-driver account
```shell
sudo adduser YourUsername
```

and add the new account to the sudoers file, so you can execute sudo commands from it
```shell
sudo usermod -aG sudo YourUsername
``` 
Logout and switch to your new user, which you'll use from here on out.

Get the most annoying thing out of the way first, tick: Firefox -> Preferences -> Restore previous session

#### 5. Set your prefered Desktop environment

Options with pre-configured settings for Kali can be seen/found using:
```shell
apt-cache search kali-desktop
```
Going with MATE currently:
```shell
sudo apt-get install kali-desktop-mate
sudo update-alternatives --config x-session-manager
# make it prettier
sudo apt-get install arc-theme
```

#### Fix Python

Currently /usr/bin/python still points to python2, which should be deprecated as much as possible by now. To drop the backwards compatibility change this symlink with this install:
```shell
sudo apt install -y python-is-python3
``` 




// obsolete

#### 5. Pick a terminal of your choosing

In general the default terminals (Kali uses Xfce as a desktop environment, which comes with Qterminal) are not the most efficient/productive environments, so depending on your current needs pick something else. Two good choices right now would be Alacritty for a GPU-enabled fast terminal (https://github.com/alacritty/alacritty) and Termite (https://github.com/thestinger/termite), which is VTE-based and a bit slower, but heavily keyboard-favored and leans on similar modes like Vim.

In my case I use termite for now, you can follow this install instructions: https://computingforgeeks.com/install-termite-terminal-on-ubuntu-18-04-ubuntu-16-04-lts/

There is a bug in the test of the vte API, so additionally you need to mini-patch one file to adjust the struct, see: https://github.com/GNOME/vte/commit/53690d5cee51bdb7c3f7680d3c22b316b1086f2c#diff-09af37e3a14d365cf086df3ead32aa7f

#### 6. Set browser defaults (for example Chromium over Firefox)
```shell
sudo update-alternatives --config x-www-browser
sudo update-alternatives --config gnome-www-browser
```





Todo:
- add command line fuzzy finder (fzf)
- x-session-manager
