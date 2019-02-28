# [TinyWeb Console for 320x240 LCD Display](https://github.com/cncjs/cncjs-pendant-tinyweb)
```
# Remove Older Downloads
rm -r cncjs-pendant-tinyweb*

# Download TinyWeb Example
wget https://github.com/cncjs/cncjs-pendant-tinyweb/releases/download/latest/cncjs-pendant-tinyweb-1.2.4-352767f.zip

# Extract Archive & Delete
unzip cncjs-pendant-tinyweb*.zip -d /home/pi/
rm -r cncjs-pendant-tinyweb*.zip

# Move / Rename Tinyweb Directory
mv /home/pi/cncjs-pendant-tinyweb* /home/pi/tinyweb

# If you are using PM2 to auto-run cncjs, you must turn that off,
# otherwise the next cncjs command will fail
pm2 stop $(which cncjs)  # stop previous instance
pm2 delete $(which cncjs)  # delete previous instance

# To start CNCjs manually with mounted TinyWeb, do this
cncjs -m /tinyweb:/home/pi/tinyweb

# If you want to use PM2 to auto-start CNCjs/TinyWeb, do this:
pm2 start $(which cncjs) -- --port 8000 -m /tinyweb:/home/pi/tinyweb
pm2 save # Set current running apps to startup
```