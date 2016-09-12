# Raspberry Pi Setup Guide

## Raspberry Pi Install: System Preparation
### Install NOOBS & RASPBIAN on your Raspberry Pi
https://www.raspberrypi.org/downloads/noobs/

### Configure Raspberry Pi
https://www.raspberrypi.org/documentation/linux/usage/users.md

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
# Update System
sudo apt-get update
sudo apt-get upgrade -y
sudo reboot

# Install Useful Tools
sudo apt-get install htop iotop nmon lsof screen git build-essential -y
```

## Install NodeJS, Node Version Manager (NVM), Node Package Manager (NPM)

### [Install Node Version Manager (NVM)](https://github.com/creationix/nvm)
https://github.com/creationix/nvm

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

### https://github.com/chovy/node-startup
```
# Install [Production Process Manager [PM2]](http://pm2.io)
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


# The Big Easy (just cut and paste)
### Configure Raspberry Pi
https://www.raspberrypi.org/documentation/linux/usage/users.md

```
# Change User Passwords
sudo passwd pi
sudo passwd root

sudo raspi-config
# Change Timezone
# Change Hostname
# Change Boot Option: Boot to CLI (No GUI)
```

### Install Everything
```
# Update System
sudo apt-get update
sudo apt-get upgrade -y

# Install Node Version Manager (NVM)
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.7/install.sh | bash

# Rerun Profile script to start NVM
source ~/.bashrc  # Rerun profile after installing nvm

# Install Node
nvm install 4  # Installs Node v4, (nvm install stable) installs Latest version of node
nvm use 4  # Sets Node to use v4

# Update Node Package Manager (NPM)
npm install npm@latest -g

# Get Version info
echo "[NPM] ============"; which npm; npm -v;
echo "[NVM] ============"; nvm --version; nvm ls
echo "[NODE] ============"; which node; node -v

# Install CNC.js
npm install -g cncjs

# Install [Production Process Manager [PM2]](http://pm2.io)
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

# Allow access to port 8000 from port 80
sudo iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8000

# Make Iptables Persistent
sudo apt-get install iptables-persistent -y
sudo sh -c "iptables-save > /etc/iptables/rules.v4"
sudo sh -c "ip6tables-save > /etc/iptables/rules.v6"
sudo service iptables-persistent start
sudo service iptables-persistent enable

# Reboot to test
sudo reboot
```


# [Wireless Setup](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)
```
# Scan for wireless networks
sudo iwlist wlan0 scan

# Open the wpa-supplicant configuration file in nano:
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf

network={
  ssid="YOUR_SSID"
  scan_ssid=1
  psk="YOUR_PASSKRY"
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