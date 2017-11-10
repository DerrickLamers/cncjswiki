Page Source: https://github.com/cncjs/cncjs.org/blob/master/pages/docs/rpi-setup-guide/index.md

# Raspberry Pi Setup Guide

We recommend that you use a [Raspberry Pi 3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/) or [Raspberry Pi 2](https://www.raspberrypi.org/products/raspberry-pi-2-model-b/) because of the performance requirements of the Node.js application. If you a buying a raspberry pi, [buy a Raspberry Pi 3](https://www.amazon.com/Raspberry-Pi-RASP-PI-3-Model-Motherboard/dp/B01CD5VC92) or latest model.

#### Recommed Software (for a full web capatable CNC software stack):
* [jscut](http://jscut.org/jscut.html) (converts SVG files to CNC cutting paths)
* [Kiri:Moto](https://grid.space/kiri/?sm:CAM) (converts 3D models to 3D mesh CNC cutting paths)

## Raspberry Pi Install: System Preparation
### Install NOOBS & RASPBIAN on your Raspberry Pi
 - https://www.raspberrypi.org/downloads/noobs/

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

#### **PAUSE HERE!!!, decide on which method to use:**
 - [Install Node.js via Package Manager](https://github.com/cncjs/cncjs/wiki/Setup-Guide:-Raspberry-Pi-%7C-Install-Node.js-via-Package-Manager-*(Recommended)*) *(Recommended)*
 - [Install Node.js via Node Version Manager (NVM)](https://github.com/cncjs/cncjs/wiki/Setup-Guide:-Raspberry-Pi-%7C-Install-Node.js-via-Node-Version-Manager-(NVM))
 - [Install Node.js Manually](https://github.com/cncjs/cncjs/wiki/Setup-Guide:-Raspberry-Pi-%7C-Install-Node.js-Manually)
 - Additional Configuration Options
     - [Wireless Setup](https://github.com/cncjs/cncjs/wiki/Setup-Guide:-Raspberry-Pi-%7C-Wireless-Setup)