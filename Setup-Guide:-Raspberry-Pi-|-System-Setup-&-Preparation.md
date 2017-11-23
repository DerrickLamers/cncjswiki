## Setup Guide: Raspberry Pi (2 & 3) | System Setup & Preparation
### Install NOOBS & RASPBIAN /w desktop on your Raspberry Pi
 - https://www.raspberrypi.org/downloads/noobs/
 - https://www.raspberrypi.org/downloads/raspbian/

### Configure Raspberry Pi
 - https://www.raspberrypi.org/documentation/linux/usage/users.md
 - https://www.raspberrypi.org/documentation/configuration/raspi-config.md

```
# Change User Passwords
sudo passwd pi
sudo passwd root

sudo raspi-config
# Change Timezone
# Change Hostname
# Change Boot Option: Boot to CLI (No GUI)
```

### Updates & Upgrade System
```
# Update System
sudo apt-get update
sudo apt-get upgrade -y
sudo apt-get dist-upgrade -y
# sudo rpi-update. # Update Raspberry Pi kernel and firmware, [is already done with 'apt-get update / upgrade'](github.com/cncjs/cncjs/issues/97)

# Install Build Essentials & GIT
sudo apt-get install -y build-essential git

# Install Useful Tools (Optional)
sudo apt-get install -y htop iotop nmon lsof screen
```

### Possible Problems
#### Slowness / File System Filled up
After I upgraded from Raspbian wheezy to Raspbian jessie, the system starting running very slowly.  Just logging in took a long time, command execution was sluggish, and after a few minutes the file system filled up and things stopped working.  It turns out that the upgrade changed the size of the swap file to a size that was much too large for the SD card.  I fixed it by editing the file /etc/dphys-swapfile (with 'sudo nano /etc/dphys-swapfile').  I changed the last line of that file from
```
#CONF-MAXSWAP=2048
```
to
```
CONF-MAXSWAP=100
```
i.e. I removed the comment character '#' and changed the size from 2048 MB (2 GB) to 100 MB.
After that change, I rebooted and, after a little time, the system stabilized and ran at its normal speed.

#### Wasted Disk Space
After upgrading and fixing the Slowness problem above, the root filesystem was still fairly full - 'df .' reported 77% full.  I fixed that with
```
$ sudo apt-get clean
$ df .
Filesystem     1K-blocks    Used Available Use% Mounted on
/dev/root        6581636 3593648   2630612  58% /
```
That removed some files in /var/cache/apt that were used during the upgrade but are no longer needed.