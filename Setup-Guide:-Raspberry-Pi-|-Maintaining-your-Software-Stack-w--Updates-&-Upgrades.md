## Maintaining your Software Stack w/ Updates & Upgrades
### Updates & Upgrade System
```
# Update System
sudo apt-get update
sudo apt-get upgrade -y  # Should also update Node.js if you used method #1
sudo apt-get dist-upgrade -y
# sudo rpi-update. # Update Raspberry Pi kernel and firmware, [is already done with 'apt-get update / upgrade'](github.com/cncjs/cncjs/issues/97)
sudo reboot
```
If you experience problems after rebooting, see [Upgrade Problems](https://github.com/cncjs/cncjs/wiki/Setup-Guide:-Raspberry-Pi-%7C-Upgrade-Problems) for possible solutions.

### Update Node Package Manager (NPM)
```
# Update Node Package Manager (NPM)
sudo npm install npm@latest -g

# Get Version info
echo "[NPM] ============"; which npm; npm -v;
echo "[NODE] ============"; which node; node -v
```

### Update CNCjs
```
# Stop CNCjs in PM2 
pm2 stop cncjs

# Update CNCjs
#sudo npm update -g cncjs --unsafe-perm  #  Tends to fail to update CNCjs, so we will reinstall CNCjs will the command bellow (no settings will be lost)
sudo npm install -g cncjs --unsafe-perm  # Install CNCjs again, if this fails or causes issue then run (sudo npm uninstall -g cncjs; sudo npm install -g cncjs --unsafe-perm )   https://github.com/cncjs/cncjs/issues/78

# Restart CNCjs in PM2
pm2 start cncjs
```

### [Update Production Process Manager [PM2]](http://pm2.keymetrics.io/docs/usage/update-pm2/)
```
# First make sure that you save all your processes:
pm2 save

Then install the latest PM2 version from NPM:
sudo npm install pm2 -g

And finally update the in-memory PM2 process:
pm2 update
```