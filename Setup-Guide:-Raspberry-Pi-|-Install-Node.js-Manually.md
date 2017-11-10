### Install Node.js Manually
Information on how to install Node.js on Raspberry Pi 3 or ARM7 compatible devices.

```
# Install Node.js (v8) [Manually] 
wget https://nodejs.org/dist/v8.9.1/node-v8.9.1-linux-armv7l.tar.xz
tar -xvf node-v8.9.1-linux-armv7l.tar.xz
cd node-v8.9.1-linux-armv7l
sudo cp -R * /usr/local/
```
Resume install at [Update Node Package Manager (NPM)](https://github.com/cncjs/cncjs/wiki/Setup-Guide:-Raspberry-Pi-%7C-Install-Node.js-via-Package-Manager-*(Recommended)*#update-node-package-manager-npm)