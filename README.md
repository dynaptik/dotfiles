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
### tmux shortcuts & cheatsheet

start new:

    tmux

start new with session name:

    tmux new -s myname

attach:

    tmux a  #  (or at, or attach)

attach to named:

    tmux a -t myname

list sessions:

    tmux ls

<a name="killSessions"></a>kill session:

    tmux kill-session -t myname

<a name="killAllSessions"></a>Kill all the tmux sessions:

    tmux ls | grep : | cut -d. -f1 | awk '{print substr($1, 0, length($1)-1)}' | xargs kill

In tmux, hit the prefix `ctrl+b` (my modified prefix is ctrl+a) and then:

## List all shortcuts
to see all the shortcuts keys in tmux simply use the `bind-key ?` in my case that would be `CTRL-B ?`

#### Sessions

    :new<CR>  new session
    s  list sessions
    $  name session

#### <a name="WindowsTabs"></a>Windows (tabs)

    c  create window
    w  list windows
    n  next window
    p  previous window
    f  find window
    ,  name window
    &  kill window

#### <a name="PanesSplits"></a>Panes (splits) 

    %  vertical split
    "  horizontal split
    
    o  swap panes
    q  show pane numbers
    x  kill pane
    +  break pane into window (e.g. to select text by mouse to copy)
    -  restore pane from window
    ⍽  space - toggle between layouts
    <prefix> q (Show pane numbers, when the numbers show up type the key to goto that pane)
    <prefix> { (Move the current pane left)
    <prefix> } (Move the current pane right)
    <prefix> z toggle pane zoom

#### <a name="syncPanes"></a>Sync Panes 

You can do this by switching to the appropriate window, typing your Tmux prefix (commonly Ctrl-B or Ctrl-A) and then a colon to bring up a Tmux command line, and typing:

```
:setw synchronize-panes
```

You can optionally add on or off to specify which state you want; otherwise the option is simply toggled. This option is specific to one window, so it won’t change the way your other sessions or windows operate. When you’re done, toggle it off again by repeating the command. [tip source](http://blog.sanctum.geek.nz/sync-tmux-panes/)


#### Resizing Panes

You can also resize panes if you don’t like the layout defaults. I personally rarely need to do this, though it’s handy to know how. Here is the basic syntax to resize panes:

    PREFIX : resize-pane -D (Resizes the current pane down)
    PREFIX : resize-pane -U (Resizes the current pane upward)
    PREFIX : resize-pane -L (Resizes the current pane left)
    PREFIX : resize-pane -R (Resizes the current pane right)
    PREFIX : resize-pane -D 20 (Resizes the current pane down by 20 cells)
    PREFIX : resize-pane -U 20 (Resizes the current pane upward by 20 cells)
    PREFIX : resize-pane -L 20 (Resizes the current pane left by 20 cells)
    PREFIX : resize-pane -R 20 (Resizes the current pane right by 20 cells)
    PREFIX : resize-pane -t 2 20 (Resizes the pane with the id of 2 down by 20 cells)
    PREFIX : resize-pane -t -L 20 (Resizes the pane with the id of 2 left by 20 cells)
    
    
#### Copy mode:

Pressing PREFIX [ places us in Copy mode. We can then use our movement keys to move our cursor around the screen. By default, the arrow keys work. we set our configuration file to use Vim keys for moving between windows and resizing panes so we wouldn’t have to take our hands off the home row. tmux has a vi mode for working with the buffer as well. To enable it, add this line to .tmux.conf:

    setw -g mode-keys vi

With this option set, we can use h, j, k, and l to move around our buffer.

To get out of Copy mode, we just press the ENTER key. Moving around one character at a time isn’t very efficient. Since we enabled vi mode, we can also use some other visible shortcuts to move around the buffer.

For example, we can use "w" to jump to the next word and "b" to jump back one word. And we can use "f", followed by any character, to jump to that character on the same line, and "F" to jump backwards on the line.

       Function                vi             emacs
       Back to indentation     ^              M-m
       Clear selection         Escape         C-g
       Copy selection          Enter          M-w
       Cursor down             j              Down
       Cursor left             h              Left
       Cursor right            l              Right
       Cursor to bottom line   L
       Cursor to middle line   M              M-r
       Cursor to top line      H              M-R
       Cursor up               k              Up
       Delete entire line      d              C-u
       Delete to end of line   D              C-k
       End of line             $              C-e
       Goto line               :              g
       Half page down          C-d            M-Down
       Half page up            C-u            M-Up
       Next page               C-f            Page down
       Next word               w              M-f
       Paste buffer            p              C-y
       Previous page           C-b            Page up
       Previous word           b              M-b
       Quit mode               q              Escape
       Scroll down             C-Down or J    C-Down
       Scroll up               C-Up or K      C-Up
       Search again            n              n
       Search backward         ?              C-r
       Search forward          /              C-s
       Start of line           0              C-a
       Start selection         Space          C-Space
       Transpose chars                        C-t

#### Misc

    d  detach
    t  big clock
    ?  list shortcuts
    :  prompt

#### Configurations Options:

    # Mouse support - set to on if you want to use the mouse
    * setw -g mode-mouse off
    * set -g mouse-select-pane off
    * set -g mouse-resize-pane off
    * set -g mouse-select-window off

    # Set the default terminal mode to 256color mode
    set -g default-terminal "screen-256color"

    # enable activity alerts
    setw -g monitor-activity on
    set -g visual-activity on

    # Center the window list
    set -g status-justify centre

    # Maximize and restore a pane
    unbind Up bind Up new-window -d -n tmp \; swap-pane -s tmp.1 \; select-window -t tmp
    unbind Down
    bind Down last-window \; swap-pane -s tmp.1 \; kill-window -t tmp

## Extending your toolbox

Check for example the tools included in https://github.com/BrashEndeavours/hotwax and pick separately (since hotbox is not really maintained any longer).

### x. Scripts


## Setting up access and playgrounds

### x. Setup HTB or any other lab with VPN connection

Download the OVPN pack, place in a folder related to the provider/labs (e.g. ~/Security/htb/)

Use the NetworkManager Applet:
1. VPN-Connections -> Add / Configure VPN ...
2. Connection Type: Import a saved VPN configuration -> choose the \*.ovpn file, accept all settings

// obsolete
