# SSH-Welcome-screen

[![Build Status](https://travis-ci.org/Habilya/SSH-Welcome-screen.svg?branch=master)](https://travis-ci.org/Habilya/SSH-Welcome-screen)
[![Latest release verison](https://img.shields.io/github/v/release/Habilya/SSH-Welcome-screen)](https://github.com/Habilya/SSH-Welcome-screen/releases)
[![License](https://img.shields.io/github/license/Habilya/SSH-Welcome-screen)](https://github.com/Habilya/SSH-Welcome-screen/blob/master/LICENSE)
[![Repository size](https://img.shields.io/github/repo-size/Habilya/SSH-Welcome-screen)](https://github.com/Habilya/SSH-Welcome-screen/releases)

[![HacktoberFest 2021](https://img.shields.io/github/hacktoberfest/2021/Habilya/SSH-Welcome-screen?color=d)](https://github.com/Habilya/SSH-Welcome-screen/labels/hacktoberfest)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat)](http://makeapullrequest.com)
[![first-timers-only](https://img.shields.io/badge/first--timers--only-friendly-blue.svg?style=flat)](https://www.firsttimersonly.com/)

[![CodeFactor](https://www.codefactor.io/repository/github/habilya/ssh-welcome-screen/badge)](https://www.codefactor.io/repository/github/habilya/ssh-welcome-screen)
[![Codacy Badge](https://api.codacy.com/project/badge/Grade/7860d5fada7e4f949d62554844ac5e41)](https://www.codacy.com/manual/Habilya/SSH-Welcome-screen?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=Habilya/SSH-Welcome-screen&amp;utm_campaign=Badge_Grade)
[![codecov](https://codecov.io/gh/Habilya/SSH-Welcome-screen/branch/master/graph/badge.svg)](https://codecov.io/gh/Habilya/SSH-Welcome-screen)

![SSH-Welcome-screen](screenshot.png)

This screen is what the server administrator ever needs. CPU temperature, Load Average, Memory and Disk usage, Available system updates, etc.

The script is **SCRIPT/MOTD.sh**

# The installation process
This screen is what the server administrator ever needs on one page.
A custom ASCII art of Montreal, a city where I live. 
CPU temperature, Load Average, Memory and Disk usage, Available system updates, that is to name a few...

Before you start, this script requires **bc** to perform some calculations, if you don't already have it, install it:

`sudo apt-get install bc`



Let's start with the number of available system updates, you need to write a simple script to be executed as root. 
If you don't have the scripts directory created, create it and a root directory, for all the scripts to be executed by root user.

`sudo mkdir -p /var/zzscriptzz/root/`

Create update_checker.sh bash script.

`sudo nano /var/zzscriptzz/root/update_checker.sh`

Paste this inside the file, save and exit (CTRL+X, Y).

```
#!/bin/bash

apt-get update
apt-get upgrade -d -y | grep 'upgraded,' | awk {'print $1'} > /var/zzscriptzz/MOTD/updates-available.dat
echo "Update Check Complete"
```

This is a script that you are going to automate with cron, it writes the number of new updates available for your operating system in this file:

```
/var/zzscriptzz/MOTD/updates-available.dat
```

Now, make this script executable.

`sudo chmod +x /var/zzscriptzz/root/update_checker.sh`


Then, configure this script to be executed by cron every 3 hours.

`sudo crontab -e`

add this line at the bottom, save and exit.
```
0 */3 * * * /var/zzscriptzz/root/update_checker.sh > /dev/null 2>&1
```

This will run the script automatically every 3 hours, so the available system updates will be checked and logged to be displayed on your welcome screen.

## The Message of the day itself
Create a directory to store the MOTD / Welcome Screen script itself. This script will be executed as a normal user, your user.

`sudo mkdir /var/zzscriptzz/MOTD`

Change the owner of the directory to your user, you are using to SSH into your machine.

`sudo chown -R [YOUR_USER]:[YOUR_USER] /var/zzscriptzz/MOTD`

Create the script

`nano /var/zzscriptzz/MOTD/MOTD.sh`

paste the contents of SCRRIPT/MOTD.sh of this repository.

Now, save, exit and make this script executable.

`chmod +x /var/zzscriptzz/MOTD/MOTD.sh`


## Applying the custom welcome message
Those commands easier to be executed as root user, so:
```
su

echo '' > /etc/motd

nano /etc/ssh/sshd_config
```

Change values of following the values displayed below.
```
PrintLastLog no
PrintMotd no
```
Now, let's restart SSH service: `/etc/init.d/ssh restart`

Now, edit this file: `nano /etc/pam.d/login`

And comment this line

```
#session optional pam_motd.so
```

Edit this file as well: `nano /etc/profile`

Add this at the end of the file: `/var/zzscriptzz/MOTD/MOTD.sh`

save CTRL+X Y.

Exit from root.

`exit`


Run the update checker for the first time

```
sudo /var/zzscriptzz/root/update_checker.sh
```

If you've done everything correctly, on every login via SSH, you should enjoy your custom, informative welcome screen.


_Note : This was developed for a Raspberry Pi, should work just fine on other debian-based system using bash shell with minor adjustments._

## Tested on
* Raspberry Pi 3 B
* Raspberry Pi 1 B
* Ubuntu 22.04.2 LTS

## Contributing
This project is participating to **Hacktoberfest** every year!
We are really excited to see your awesome PRs.
Contributions for Hacktoberfest are welcome 🎉🎉

## Contributors
[![Contributors](https://img.shields.io/github/contributors/Habilya/SSH-Welcome-screen)](https://github.com/Habilya/SSH-Welcome-screen/graphs/contributors)

Hail to all elegant people contributing to the project!

|   |   |   |   |   |   |   |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|<a href="https://github.com/Habilya"><img src="https://avatars1.githubusercontent.com/u/26153731?v=3" width="100px;" alt=""/><br /><sub><b>Habilya</b></sub></a>|<a href="https://github.com/k4ch0"><img src="https://avatars1.githubusercontent.com/u/1848897?v=3" width="100px;" alt="k4ch0"/><br /><sub><b>k4ch0</b></sub></a>|<a href="https://github.com/ccsplit"><img src="https://avatars1.githubusercontent.com/u/2328721?v=3" width="100px;" alt="ccsplit"/><br /><sub><b>ccsplit</b></sub></a>|<a href="https://github.com/watermelon1995"><img src="https://avatars1.githubusercontent.com/u/12808978?v=3" width="100px;" alt="watermelon1995"/><br /><sub><b>watermelon1995</b></sub></a>|<a href="https://github.com/tvollscw"><img src="https://avatars1.githubusercontent.com/u/40273125?v=3" width="100px;" alt="tvollscw"/><br /><sub><b>tvollscw</b></sub></a>|<a href="https://github.com/tehtbl"><img src="https://avatars1.githubusercontent.com/u/3999809?v=3" width="100px;" alt="tehtbl"/><br /><sub><b>tehtbl</b></sub></a>|<a href="https://github.com/javier-lopez"><img src="https://avatars1.githubusercontent.com/u/75626?v=3" width="100px;" alt="javier-lopez"/><br /><sub><b>javier-lopez</b></sub></a>|
|<a href="https://github.com/estevao90"><img src="https://avatars1.githubusercontent.com/u/18039589?v=3" width="100px;" alt="estevao90"/><br /><sub><b>estevao90</b></sub></a>|<a href="https://github.com/a7r3"><img src="https://avatars1.githubusercontent.com/u/14874906?v=3" width="100px;" alt="a7r3"/><br /><sub><b>a7r3</b></sub></a>|<a href="https://github.com/RytoEX"><img src="https://avatars1.githubusercontent.com/u/624931?v=3" width="100px;" alt="RytoEX"/><br /><sub><b>RytoEX</b></sub></a>|<a href="https://github.com/ccdlus"><img src="https://avatars1.githubusercontent.com/u/25654877?v=3" width="100px;" alt="RytoEX"/><br /><sub><b>ccdlus</b></sub></a>|


Cheers!
