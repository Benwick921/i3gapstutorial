# i3-gaps Installation Tutorial

*Update 22/09/2021*

Parts of the tutorial are taken from other tutorials.<br/>
I put my dot files to take inspiration from.

![GitHub Logo](https://cdn.discordapp.com/attachments/537387570620006401/698175766772318218/unknown.png)

## Install from Repository
i3-gaps can be installed directly from repository, no need to mess with dependecies and manual installing pain. Add the kali linux repository.

```
deb http://http.kali.org/kali kali-rolling main contrib non-free
```

If there is an error about a missing key similar to the following one:
```
W: GPG error: http://http.kali.org/kali kali-rolling InRelease. The following signatures couldn't be verified because the public key is not available: NO_PUBKEY F6BC817356A3D45E
```

Execute the following command with the key specified in the error message:

```
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys <MISSING KEY>
```

and finally update and install i3-gaps.

```
sudo apt update
sudo apt install i3-gaps
```

Manually installing i3-gaps is not suggested because many libraries has changes and they dont work any more and many repositories doesent have the libraries needed.

## Manual Installaion's dependencies
i3-gaps has some packages that are required for it to work so install them:
```bash
sudo apt install libxcb1-dev libxcb-keysyms1-dev libpango1.0-dev libxcb-util0-dev libxcb-icccm4-dev
sudo apt install libyajl-dev libstartup-notification0-dev libxcb-randr0-dev libev-dev libxcb-cursor-dev
sudo apt install libxcb-xinerama0-dev libxcb-xkb-dev libxkbcommon-dev libxkbcommon-x11-dev autoconf
sudo apt install xutils-dev libtool automake
```
You also need to install libxcb-xrm-dev in case its missing.
``` bash
sudo apt install libxcb-shape0-dev
sudo apt install libxcb-xrm-dev
``` 

## Installing
i3-gaps also needs to be installed from source so run these commands (use sudo just in case):
``` bash
cd tmp
git clone https://www.github.com/Airblader/i3 i3-gaps
cd i3-gaps
git checkout gaps && git pull
sudo autoreconf --force --install
sudo rm -rf build
mkdir build
cd build
sudo ../configure --prefix=/usr --sysconfdir=/etc --disable-sanitizers
sudo make
sudo make install
```
Now i3-gaps should be installed.

## Configuring
To enable gaps you need to set some variables in your i3 config:
```
gaps inner <# of pixels>
gaps outer <# of pixels>
```
I suggest to add the previews two lines at the bottom of your i3 config file to keep it organized from what's default and what you are adding, your i3 config file should be in:
```
~/.config/i3/config
```
If you don't find it you can create it manually.
Add this to get rid of titlebars because gaps doesen't work with titlebars:
```
for_window [class="^.*"] border pixel 2
```
Refresh i3 (Mod+r) and you're good to go!

## i3-gaps Troubleshooting
ERROR: Dependency: i3 Error: Status_command not found (exit 127).
``` bash
sudo apt install i3status
```
ERROR: Mod+d not working.
``` bash
sudo apt-get install dmenu
```

## Desktop Background
To change the desktop background we need to install the following program:
``` bash
sudo apt install feh
```
and add the following line to your i3 config file:
``` 
exec_always feh --bg-scale /path/to/image
```
now refresh i3 (Mod+r).

## Terminal tranbsparency
In order to get ther fancy terminal trasparency `compon` is needed. Follow the guide to install and set compton config file.

Install `compotn` (updating is suggested before installing)

```
sudo apt install compton
```

Create a config file for compton to the following location

```
~/.config/compton.conf
```

Put this line inside the config file and replace with your terminal.

```
opacity-rule = ["85:class_g = 'Gnome-terminal'"];
```

Add the execution of compton on thr i3 config file.

```
exec --no-startup-id compton
```
Now the trasparency is set to default. You can change the active and inactive window trasparency level with the following lines.

```
active-opacity = 0.75;
inactive-opacity = 0.75;
```

In order to see the trasparency now (at least for Gnome-terminal) I can set it from the terminal preferences menu and chose what level of transparency I want without messing up with the configuration file of compton.

![lvl1](https://github.com/Benwick921/i3gapstutorial/blob/master/lvl1.PNG)
![lvl2](https://github.com/Benwick921/i3gapstutorial/blob/master/lvl2.PNG)

In order to see the result from compton config file you have to exit i3 and log in back.

## Enable copy/paste between host and i3-vm

Install open-vm-tools

```
sudo apt install open-vm-tools
sudo apt install open-vm-tools-desktop
```

add the following line to the i3 config file and reboot.

```
exec --no-startup-id vmware-user
```

If not working try unistalling vmware-tools installed via vmware.

## System Troubleshooting
Changing default terminal bind in `$Mod+d exec urxvt` doesent work any more.
For more information follow the tutorial [here](https://www.osradar.com/change-the-default-terminal-emulator-on-linux/).
The solution is to install `URxvt`.
``` bash
sudo apt install rxvt-unicode
```
and change the whole system's default terminal.
``` bash
sudo update-alternatives --config x-terminal-emulator
```
then customize your `~/.Xresources` file to customize your `URxvt` terminal.<br/>
Sometimes i3-gaps doesen't start the network manager for some reason and you are disconnected, you need to restart the network manager.
``` bash
sudo service network-manager restart
```
