### [Install Node.js via Package Manager](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)
```
# Install Node.js via Package Manager & Add Package Source
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -  # Install NodeJS v10
sudo apt-get install -y nodejs  # npm nodejs-legacy #(Installed with nodesource)

# Optional: install build tools
# To compile and install native addons from npm you may also need to install build tools:
sudo apt-get install -y build-essential
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

#### Install Specific Version of CNCjs
#sudo npm install -g cncjs@v1.9.0-beta.1 --unsafe-perm  # Installs Specific Version based on TAG
```

* Possible Install Problems & Solutions:

  * ##### [JSON ERROR](https://github.com/cncjs/cncjs/issues/326)
    * PROBLEM: Fails at `sudo npm install -g cncjs@latest --unsafe-perm`
    * SOLUTION: Try updating npm installer itself to the latest version (6.x), and removing the cache (~/.npm/) before installing CNCjs again. 
      ```
      npm i -g npm
      $ rm -rf ~/.npm/
      ```
  * ##### ETIMEDOUT
    * PROBLEM: My 'npm install' runs frequently fail with 'ETIMEDOUT' errors, like this:
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
    
    * SOLUTION: Sometimes simply re-executing the 'npm install' commands works, but the following solution has also been suggested (see https://github.com/npm/npm/blob/5b4e3f3caff1f7ed30281b0b450de056a817bbc5/CHANGELOG.md#limit-concurrent-requests for more info):
      ```
      $ npm set maxsockets 3
      ```
      Then re-execute the 'npm install' command.  Supposedly the problem is that npm normally tries to fetch a lot of things at the same time, using say 50 simultaneous network connections, and that overloads some network routers.  Setting maxsockets to 3 makes it less aggressive and thus more likely to succeed.  When I tried it, the 'npm install' succeeded, and seemed to work faster than before.  I suspect that the Raspberry Pi does not have enough memory and CPU resources to run a lot of simultaneous connections efficiently.

### Test the Installation

At this point you should run a quick test to see if things are working.  Run this command:

```
cncjs --port 8000
```
Something like this should be displayed in the terminal:
```
2019-10-04T22:45:16.701Z - info init Loading configuration from "/home/pi/.cncrc"
2019-10-04T22:45:17.931Z - info init Starting the server at http://127.0.1.1:8000
```
That means the CNCjs server is running, ready to accept connections from a browser.  Start the Chromium browser on your Pi (other browsers than Chromium will probably work too) and enter the URL "127.0.1.1:8000" (alternatively, you can write that as "localhost:8000").  It should say "Loading" and after a few seconds the CNCjs user interface should appear in the browser.

To stop the CNCjs server, type ^C (Ctrl-C) in the terminal.

If your Pi is headless (no graphics display), you can connect from a browser on a different machine on the same network as the Pi.  To find the Pi's external IP address, you can use the command `hostname -I`.  Let's assume that the Pi's external IP address is "192.168.2.23" - then you would browse to "192.168.2.23:8000" from that other machine.  (If you had to stop the server with ^C in order to run the hostname command, you will need to restart it with the `cncjs --port 8000` command.)

* Possible Install Problems & Solutions:

  * #### Serialport version problem

   You might see an error message like this:
   ```
   Error: Error: The module '/usr/lib/node_modules/cncjs/node_modules/serialport/build/Release/serialport.node'
   was compiled against a different Node.js version using NODE_MODULE_VERSION 57. This version of Node.js
   requires NODE_MODULE_VERSION 64. Please try re-compiling or re-installing the module (for instance,
   using `npm rebuild` or `npm install`).
   ```
   To fix that, you can do this:
   ```
   cd /usr/lib/node_modules/cncjs/node_modules
   sudo npm rebuild --update-binary--unsafe-perm 
   ```
   There should be many lines of output as it recompiles the serialport code.  When it finishes, you can retry the `cncjs --port 8000` command and connect from a browser.

After you get a successful test, kill the CNCjs server by typing ^C (Ctrl-C) and proceed to the next step, whose purpose is to make the CNCjs server start automatically every time you reboot the Pi.  Note that starting the server *does not* start the user interface in a browser.  Autostarting the browser requires an additional step.

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

### Autostarting the browser

To make the Chromium browser start automatically, and display the CNCjs user interface, do this:

```
cd /home/pi

cat >startCNCjsUI <<EOF
#!/bin/bash

# Prevent the screen from turning off
xset s off
xset -dpms

# Show the user why it is taking so long
zenity --info --no-wrap --timeout 240 --width=400 --height=400 --text="Waiting for CNCjs server to start" &

# Wait until the CNCjs server becomes responsive before starting the browser
until $(curl --output /dev/null --silent --head --fail http://localhost:8000); do
  sleep 1
done

# Finally, start the browser, pointing to the CNCjs server
# --kiosk makes the browser occupy the entire screen.
# If you want to kill the full-screen browser, use ALT-F4
# If you omit --kiosk, the browser will start in a normal window
chromium-browser --kiosk http://localhost:8000
EOF

chmod a+x startCNCjsUI

mkdir -p .config/lxsession/LXDE-pi

cat >.config/lxsession/LXDE-pi/autostart <<EOF
@lxpanel --profile LXDE-pi
@pcmanfm --desktop --profile LXDE-pi
/home/pi/startCNCjsUI
EOF
```
That does two things
* It creates an executable script named _startCNCjsUI_ that waits for the cncJS server to finish starting, then starts a full-screen browser to talk to the server.
* It creates an autostart file _.config/lxsession/LXDE-pi/autostart_ to override the system-supplied startup file for the windowing environment.  The new autostart file invokes the _startCNCjsUI_ script.

The net result is that the CNCjs user interface will start automatically and take over the screen, as soon as the system is ready.
### Reboot to test
```sudo reboot```