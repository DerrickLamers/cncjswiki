### [Install Node.js via Package Manager](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)
```
# Install Node.js via Package Manager & Add Package Source
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -  # Install NodeJS v8
sudo apt-get install -y nodejs  # npm nodejs-legacy #(Installed with nodesource)
```

### Update Node Package Manager (NPM)
```
# Update Node Package Manager (NPM)
sudo npm install npm@latest -g

# Get Version info
echo "[NPM] ============"; which npm; npm -v;
echo "[NODE] ============"; which node; node -v
```

~~### Install Node.JS Serial Port application first (OPTIONAL)
```npm install serialport```~~

### Install CNCjs
```
# Install Latest Release Version of CNCjs
sudo npm install -g cncjs@latest --unsafe-perm

# --- OR ---

# Install Specific Version of CNCjs
#sudo npm install -g cncjs@v1.9.0-beta.1 --unsafe-perm  # Installs Specific Version based on TAG
```
#### Possible Problem - ETIMEDOUT
My 'npm install' runs frequently fail with 'ETIMEDOUT' errors, like this:

```
npm ERR! code ETIMEDOUT
npm ERR! errno ETIMEDOUT
npm ERR! network request to https://registry.npmjs.org/@trendmicro/react-paginations/-/react-paginations-0.6.1.tgz failed, reason: connec t ETIMEDOUT 151.101.24.162:443
npm ERR! network This is a problem related to network connectivity.
npm ERR! network In most cases you are behind a proxy or have bad network settings.
npm ERR! network
npm ERR! network If you are behind a proxy, please make sure that the
npm ERR! network 'proxy' config is set properly.  See: 'npm help config'

npm ERR! A complete log of this run can be found in:
npm ERR!     /root/.npm/_logs/2017-11-23T18_29_57_208Z-debug.log
```
Sometimes simply re-executing the 'npm install' commands works, but the following solution has also been suggested (see https://github.com/npm/npm/blob/5b4e3f3caff1f7ed30281b0b450de056a817bbc5/CHANGELOG.md#limit-concurrent-requests for more info):
```
$ npm set maxsockets 3
```
Then re-execute the 'npm install' command.  Supposedly the problem is that npm normally tries to fetch a lot of things at the same time, using say 50 simultaneous network connections, and that overloads some network routers.  Setting maxsockets to 3 makes it less aggressive and thus more likely to succeed.  When I tried it, the 'npm install' succeeded, and seemed to work faster than before.  I suspect that the Raspberry Pi does not have enough memory and CPU resources to run a lot of simultaneous connections efficiently.

### Install [Production Process Manager [PM2]](http://pm2.io)
```
# Install PM2
sudo npm install -g pm2

# Setup PM2 Startup Script
# sudo pm2 startup  # To Start PM2 as root
pm2 startup  # To start PM2 as pi / current user
  #[PM2] You have to run this command as root. Execute the following command:
  sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u pi --hp /home/pi

# Start CNCjs (on port 8000, /w Tinyweb mount point) with PM2
pm2 start $(which cncjs) -- --port 8000 -m /tinyweb:/home/pi/tinyweb

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

# How-to: Save & Reload Rules
#sudo netfilter-persistent save
#sudo netfilter-persistent reload

# How-to: Manually Save Rules
#sudo sh -c "iptables-save > /etc/iptables/rules.v4"
#sudo sh -c "ip6tables-save > /etc/iptables/rules.v6"

# Run this if issues to reconfigure iptables-persistent
# sudo dpkg-reconfigure iptables-persistent
```

### Reboot to test
```sudo reboot```