# [TinyWeb Console for 320x240 LCD Display](https://github.com/cncjs/cncjs/wiki/User-Guide#tinyweb-console-for-320x240-lcd-display)
```
# Remove Older Downloads
rm -r cncjs-pendant-tinyweb*

# Download TinyWeb Example
wget https://github.com/cncjs/cncjs-pendant-tinyweb/releases/download/latest/cncjs-pendant-tinyweb-1.0.0-613f598.zip

# Extract Archive & Delete
unzip cncjs-pendant-tinyweb*.zip -d /home/pi/
rm -r cncjs-pendant-tinyweb*.zip

# Move / Rename Tinyweb Directory
mv /home/pi/cncjs-pendant-tinyweb* /home/pi/tinyweb

# How-to Start CNCjs w/ mounted TinyWeb
cncjs -m /tinyweb:/home/pi/tinyweb

# Start CNCjs (on port 8000, /w Tinyweb) with PM2
pm2 stop cncjs  # stop pervious instance
pm2 delete cnsjc  # delete pervious instance
pm2 start $(which cncjs) -- --port 8000 -m /tinyweb:/home/pi/tinyweb
pm2 save # Set current running apps to startup
```