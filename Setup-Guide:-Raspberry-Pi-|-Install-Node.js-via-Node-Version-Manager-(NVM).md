## [Install Install Node.js via Node Version Manager (NVM)](https://github.com/creationix/nvm)
### [Install Node Version Manager (NVM)](https://github.com/creationix/nvm)

```
# Install Node Version Manager (NVM)
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.6/install.sh | bash

# Rerun Profile script to start NVM
source ~/.bashrc  # Rerun profile after installing nvm
```

### [Install Node.js via NVM](https://nodejs.org)
Installing an ARM-version of Node has become very easy:

```
# Install Node.js using Node Version Manager
nvm install 8  # Installs Node v8, (nvm install stable) installs Latest version of node
nvm use 8  # Sets Node to use v8
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

~~### Install Node.JS Serial Port application first (OPTIONAL)
```npm install serialport```~~

### Install CNCjs
```npm install -g cncjs```

### Allow access to port 8000 from port 80
```
# Allow access to port 8000 from port 80
sudo iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8000

# Make Iptables Persistent
sudo apt-get install iptables-persistent -y

# How-to: Save & Reload Rules
#sudo netfilter-persistent save
#sudo netfilter-persistent reload

# How-to: Manually Save Rules
#sudo sh -c "iptables-save > /etc/iptables/rules.v4"
#sudo sh -c "ip6tables-save > /etc/iptables/rules.v6"

# Run this if issues to reconfigure iptables-persistent
# sudo dpkg-reconfigure iptables-persistent
```


## Auto Start Options

### Install [Production Process Manager [PM2]](http://pm2.io)
```
# Install Production Process Manager [PM2]
npm install pm2 -g

# Start CNCjs (on port 8000) with PM2
pm2 start $(which cncjs) -- --port 8000

# Setup PM2 Startup Script
pm2 startup debian
#[PM2] You have to run this command as root. Execute the following command:
#sudo su -c "env PATH=$PATH:/home/pi/.nvm/versions/node/v{NODE-VERSION}/bin pm2 startup debian -u pi --hp /home/pi"
sudo su -c "env PATH=$PATH:/home/pi/.nvm/versions/node/v6.5.0/bin pm2 startup debian -u pi --hp /home/pi"


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