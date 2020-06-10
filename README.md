# dotfiles to setup my pentesting environment

## The OS

Start by getting the latest pentesting OS of your choice, currently its still Kali Linux for me, but for you it might be parrot or one of the Arch Linux variants.

At the time of this writing I pulled from OffSec the VMware image of Kali: Kali-Linux-2020.2-vmware-amd64

When starting for the first time, you chose "I copied it" when VMware asks where this image is from. 

!!! I do not recommend using VMWare Workstation Player, as it does not include the snapshot functionality any longer. Either go with Pro, or switch to VirtualBox.

## First steps

1. Update and upgrade the system by running:
```shell
sudo apt update && sudo apt dist-upgrade -y
```

2. (optional) Change your keyboard layout - in my case: German layout, by running:
```shell
sudo dpkg-reconfigure keyboard-configuration
```

Afterwards restart the service so it takes effect:
```shell
sudo service keyboard-setup restart
```

But in many cases a full reboot is required.

3. Change the default password for the "kali" user to something cryptic:
```shell
passwd
```

4. Add a lowpriv daily-driver account
```shell
sudo adduser YourUsername
```

and add the new account to the sudoers file, so you can execute sudo commands from it
```shell
sudo usermod -aG sudo YourUsername
``` 
