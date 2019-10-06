**pm2** is a application that manages other applications, doing things like starting those other applications automatically, restarting them if they crash, etc.  pm2 itself uses the node.js JavaScript framework - the same framework that CNCjs uses.  pm2 can be used to automatically start the CNCjs server, but there is another way to do it (**cron**) that is smaller, faster, and simpler.  cron is included by default in the Rasbian Linux distribution, so there is no need to download/install any new packages.  It is faster because it is written directly in C and thus does not incur the extra overhead of starting node.js and loading a lot of JavaScript code (also, cron is a standard part of Linux startup so it is already running anyway, long before the system is ready for CNCjs startup). It is simpler because you only have to add one line to a file, instead of learning several pm2 commands.

For those reasons, cron is the recommended way to auto-start the CNCjs server on a Raspberry Pi.

That said, if you prefer to use pm2 to start the CNCjs server, here is how.

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
