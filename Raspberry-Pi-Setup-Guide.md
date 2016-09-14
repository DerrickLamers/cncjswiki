# Raspberry Pi Setup Guide
I recommend that you use a [Raspberry Pi 3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b/) or [Raspberry Pi 2](https://www.raspberrypi.org/products/raspberry-pi-2-model-b/) because of the performance requirements of the Node.js application. If you a buying a raspberry pi, [buy a Raspberry Pi 3](https://www.amazon.com/Raspberry-Pi-RASP-PI-3-Model-Motherboard/dp/B01CD5VC92) or latest model.

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

### Updates & Upgrades
```
 Update System
sudo apt-get update
sudo apt-get upgrade -y

# Install Build Essentials & GIT
sudo apt-get install -y build-essential git

# Install Useful Tools (Optional)
sudo apt-get install htop iotop nmon lsof screen -y
```

#### **STOP HERE, decide on which method to use:**
 - [Install Node.js via Package Manager](#install-nodejs-via-package-manager)
 - [Install Install Node.js via Node Version Manager (NVM)](#install-install-nodejs-via-node-version-manager-nvm)
 - Additional Configuration Options
	 - [Wireless Setup](#wireless-setup)

---------

### [Install Node.js via Package Manager](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)
```
curl -sL https://deb.nodesource.com/setup_4.x | sudo -E bash -
sudo apt-get install -y nodejs  # npm nodejs-legacy #(Installed with nodesource)

# Update Node Package Manager (NPM)
sudo npm install npm@latest -g

# Get Version info
echo "[NPM] ============"; which npm; npm -v;
echo "[NODE] ============"; which node; node -v
```

### Install Serial Port (Optional)
```#sudo npm install -g serialport --unsafe-perm  # --fallback-to-build```

### Install CNC.js
```sudo npm install -g cncjs --unsafe-perm```

### Install [Production Process Manager [PM2]](http://pm2.io)
```
# Install PM2
sudo npm install -g pm2

# Setup PM2 Startup Script
# sudo pm2 startup  # To Start PM2 as root
pm2 startup  # To start PM2 as pi / current user
  #[PM2] You have to run this command as root. Execute the following command:
  sudo su -c "env PATH=$PATH:/usr/bin pm2 startup linux -u pi --hp /home/pi"

# Start CNC.js (on port 8000) with PM2
pm2 start $(which cnc) -- --port 8000

# Set current running apps to startup
pm2 save

# Get list of PM2 processes
pm2 list
```

### Iptables (allow access to port 8000 from port 80)
```
# Iptables (allow access to port 8000 from port 80)
sudo iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8000

# Make Iptables Persistent
sudo apt-get install iptables-persistent -y
sudo sh -c "iptables-save > /etc/iptables/rules.v4"
sudo sh -c "ip6tables-save > /etc/iptables/rules.v6"
sudo service iptables-persistent start
sudo service iptables-persistent enable
# sudo dpkg-reconfigure iptables-persistent # Run this if issues to reconfigure iptables-persistent
```

### Reboot to test
```sudo reboot```

#### **You're Done, STOP HERE**
The information below is a breakdown of the process above with different / additional options as part of a operate process.

-----------

## [Install Install Node.js via Node Version Manager (NVM)](https://github.com/creationix/nvm)
### [Install Node Version Manager (NVM)](https://github.com/creationix/nvm)

```
# Install Node Version Manager (NVM)
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.7/install.sh | bash

# Rerun Profile script to start NVM
source ~/.bashrc  # Rerun profile after installing nvm
```

### [Install Node.js](https://nodejs.org)
Installing an ARM-version of Node has become very easy:

```
# Install Node.js using Node Version Manager
nvm install 4  # Installs Node v4, (nvm install stable) installs Latest version of node
nvm use 4  # Sets Node to use v4
```

### [Update Node Package Manager (NPM)](https://docs.npmjs.com/getting-started/installing-node)
```npm install npm@latest -g```

To make sure it ran correctly, run ``npm -v``, ``nvm --version``, & ``node -v``. It should return the current versions.

```
# Output Node Related Version Info
echo "[NPM] ============"; which npm; npm -v;
echo "[NVM] ============"; nvm --version; nvm ls
echo "[NODE] ============"; which node; node -v
```


## Install CNC.js

~~### Install Node.JS Serial Port application first (OPTIONAL)
```npm install serialport```~~

### Install CNC.js
```npm install -g cncjs```

### Upgrade CNC.js
```npm update -g cncjs```

### Allow access to port 8000 from port 80
```
# Allow access to port 8000 from port 80
sudo iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8000

# Make Iptables Persistent
sudo apt-get install iptables-persistent -y
sudo sh -c "iptables-save > /etc/iptables/rules.v4"
sudo sh -c "ip6tables-save > /etc/iptables/rules.v6"
sudo service iptables-persistent start
sudo service iptables-persistent enable
```


## Auto Start Options

### Install [Production Process Manager [PM2]](http://pm2.io)
```
# Install Production Process Manager [PM2]
npm install pm2 -g

# Start CNC.js (on port 8000) with PM2
pm2 start /home/pi/.nvm/versions/node/v4.5.0/bin/cnc -- --port 8000

# Setup PM2 Startup Script
pm2 startup debian
#[PM2] You have to run this command as root. Execute the following command:
sudo su -c "env PATH=$PATH:/home/pi/.nvm/versions/node/v4.5.0/bin pm2 startup debian -u pi --hp /home/pi"

# Set current running apps to startup
pm2 save

# Get list of PM2 processes
pm2 list
```

### Auto Start on Boot with cron (Alternative)
```
# Open crontab
crontab -u pi -e

# Paste the following into Cron Tab [ NOTE: which cnc  # used to find location of application ]
@reboot env PATH=$PATH:/home/pi/.nvm/versions/node/v4.5.0/bin /home/pi/.nvm/versions/node/v4.5.0/bin/cnc >> $HOME/cncjs.log 2>&1
```

#### **You're Done, STOP HERE**

---------


# [Wireless Setup](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)
```
# Scan for wireless networks
sudo iwlist wlan0 scan

# Open the wpa-supplicant configuration file in nano:
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf

network={
  ssid="YOUR_SSID"
  scan_ssid=1
  psk="YOUR_PASSKEY"
  mode=0
  proto=WPA2
  key_mgmt=WPA-PSK
  pairwise=CCMP
  group=CCMP
  auth_alg=OPEN
  id_str="raspi"
  priority=1
}

# Restart Interface
sudo ifdown wlan0
sudo ifup wlan0

# Check Interface for IP
ifconfig wlan0
```