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

#### 6. Fix Python

Currently /usr/bin/python still points to python2, which should be deprecated as much as possible by now. To drop the backwards compatibility change this symlink with this install:
```shell
sudo apt install -y python-is-python3
``` 

#### 7. Get notetaking sorted out

Currently I opt for Obsidian, which comes in a AppImage packaging. For ease install a current version of appimager launcher: https://github.com/TheAssassin/AppImageLauncher/releases

```shell
sudo dpkg -i appimagelauncher_<version>.bionic_amd64.deb
```

Get Obsidian from: https://obsidian.md/ - place in a folder like ~/Applications and double click it, follow the instructions of AppImage Launcher, and hit "integrate and run".

For screenshots I go with Flameshot:
```shell
sudo apt install flameshot
```

To copy terminal outputs easily into Obsidian for your notes, use xclip:
```shell
sudo apt install xclip
```

#### 8. tmux time
Edit your .tmux.conf in ~
```bash

```

#### 9. Setup HTB or any other lab with VPN connection

Create OVPN pack (EU Free -> any gateway -> TCP)

Save the OpenVPN configuration pack (\*.ovpm) and use the NetworkManager Applet:
1. VPN-Connections -> Add / Configure VPN ...
2. Connection Type: Import a saved VPN configuration -> choose the \*.ovpn file, accept all settings

// obsolete
