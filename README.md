# dotfiles to setup my pentesting environment

## The OS

Start by getting the latest pentesting OS of your choice, currently its still Kali Linux for me, but for you it might be parrot or one of the Arch Linux variants.

At the time of this writing I pulled from OffSec the VMware image of Kali: Kali-Linux-2020.2-vmware-amd64

When starting for the first time, you chose "I copied it" when VMware asks where this image is from. 

!!! I do not recommend using VMWare Workstation Player, as it does not include the snapshot functionality any longer. Either go with Pro, or switch to VirtualBox.

## First steps

### 1. Update and upgrade the system by running:
```shell
sudo apt update && sudo apt dist-upgrade -y
```

### 2. (optional) Change your keyboard layout - in my case German - by running:
```shell
sudo dpkg-reconfigure keyboard-configuration
```

Afterwards restart the service so it takes effect:
```shell
sudo service keyboard-setup restart
```

But in many cases a full reboot is required.

### 3. Change the default password for the "kali" user to something cryptic:
```shell
passwd
```

### 4. Add a lowpriv daily-driver account
```shell
sudo adduser YourUsername
```

and add the new account to the sudoers file, so you can execute sudo commands from it
```shell
sudo usermod -aG sudo YourUsername
``` 
Logout and switch to your new user, which you'll use from here on out.

Get the most annoying thing out of the way first, tick: Firefox -> Preferences -> Restore previous session

### 5. Set your prefered Desktop environment

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

### 6. Fix Python

Currently /usr/bin/python still points to python2, which should be deprecated as much as possible by now. To drop the backwards compatibility change this symlink with this install:
```shell
sudo apt install -y python-is-python3
``` 

### 7. Get notetaking sorted out

Currently I opt for Obsidian, which comes in a AppImage packaging. For ease install a current version of appimager launcher: https://github.com/TheAssassin/AppImageLauncher/releases

```shell
sudo dpkg -i appimagelauncher_<version>.bionic_amd64.deb
```

Get Obsidian from: https://obsidian.md/ - place in a folder like ~/Applications and double click it, follow the instructions of AppImage Launcher, and hit "integrate and run".

Alternative: Notion

```shell
sudo apt install p7zip-full imagemagick fakeroot make g++
wget https://notion.davidbailey.codes/notion-linux.list
sudo mv notion-linux.list /etc/apt/sources.list.d/notion-linux.list
sudo apt update && sudo apt install notion-desktop
# or
sudo apt update && sudo apt install notion-enhanced
# for the modded version
```

For screenshots I go with Flameshot:
```shell
sudo apt install flameshot
```

To copy terminal outputs easily into Obsidian for your notes, use xclip:
```shell
sudo apt install xclip
```

### 8. Configure your shell / bash

Get coloring sorted:
```shell
sudo apt install grc
```

Fix defaults in .bashrc

```shell



```

Set appropriate aliases in .bash_aliases
```shell
alias nmap="grc nmap"
alias findvm="grc nmap -sn -sV 192.168.1.0/24 | grep -E -o '(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)' | sort"
alias thmvpn="openvpn ~/tryhackme/deezysec.ovpn"
alias htbvpn="openvpn ~/htb/lab_deezysec.ovpn"
alias enum4linux="/opt/enum4linux-ng/enum4linux-ng.py"
alias wgetfiles="echo wget -nd -np -R "index.html*" -P /tmp/rowbot --recursive http:// | xclip"
alias getip="ip addr show tun0 | grep -Po 'inet \K[\d.]+' | xclip"
alias grep="grep --color=auto"
alias egrep="egrep --color=auto"
alias fgrep="fgrep --color=auto"
```


### x. tmux time - or Terminator?
Edit your .tmux.conf in ~
```bash

```
## Extending your toolbox

Check for example the tools included in https://github.com/BrashEndeavours/hotwax and pick separately (since hotbox is not really maintained any longer).

### x. Scripts


## Setting up access and playgrounds

### x. Setup HTB or any other lab with VPN connection

Create OVPN pack (EU Free -> any gateway -> TCP)

Save the OpenVPN configuration pack (\*.ovpm) and use the NetworkManager Applet:
1. VPN-Connections -> Add / Configure VPN ...
2. Connection Type: Import a saved VPN configuration -> choose the \*.ovpn file, accept all settings

// obsolete
