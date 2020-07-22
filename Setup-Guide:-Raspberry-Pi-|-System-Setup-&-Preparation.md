## Setup Guide: Raspberry Pi (2 & 3) | System Setup & Preparation
### Install NOOBS & RASPBIAN /w desktop on your Raspberry Pi
 - https://www.raspberrypi.org/downloads/noobs/
 - https://www.raspberrypi.org/downloads/raspbian/

### Configure Raspberry Pi
 - https://www.raspberrypi.org/documentation/linux/usage/users.md
 - https://www.raspberrypi.org/documentation/configuration/raspi-config.md

In the instructions below, do not directly type lines that begin with # - they are comments that you should read.

```
# Change User Passwords
sudo passwd pi
sudo passwd root

sudo raspi-config
# Change Timezone
# Change Hostname
# Change Boot Option: Boot to CLI (No GUI)

# Install Build Essentials & GIT
sudo apt-get install -y build-essential git

# Install Useful Tools (Optional)
sudo apt-get install -y htop iotop nmon lsof screen

# Reboot
sudo reboot
```

If you experience problems after rebooting, see [Upgrade Problems](https://github.com/cncjs/cncjs/wiki/Setup-Guide:-Raspberry-Pi-%7C-Upgrade-Problems) for possible solutions.

