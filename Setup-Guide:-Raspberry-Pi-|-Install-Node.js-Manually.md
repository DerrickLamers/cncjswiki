### Install Node.js Manually
Information on how to install Node.js on Raspberry Pi computers.

Go to https://nodejs.org/en/download/ and download the latest version, selecting the ARM version that corresponds to your Raspberry Pi model.  Pi Zero (W) and Pi 1 use ARMv6, Pi 2, Pi 3 and Pi 4 use ARMv7.  If in doubt, type this command

```
cat /proc/cpuinfo
```

and look at the first line that begins with "model name".  (The newer Pis beginning with Pi 2 v1.2 actually have ARMv8 processors that could support 64-bit operating systems, but at the time of this writing, all Pis run a 32-bit operating system so, for the ARMv8 processors, the architecture version is downgraded in software to ARMv7).

Transfer the downloaded file to your Raspberry Pi if necessary.  A good place to put it is in your home directory, for example "/home/pi". 

The file that you download will have a name like node-**v10.16.3**-linux-**armv6l**.tar.xz where the parts in boldface depend on the version and ARM architecture.  In the directory containing the downloaded file, type the following commands, substituting the appropriate values for the boldface parts of the name.

```
# Install Node.js [Manually] 
tar -xvf xvf node-v10.16.3-linux-armv6l.tar.xz
cd node-v10.16.3-linux-armv6l
sudo cp -R * /usr/local/
```
Resume install at [Update Node Package Manager (NPM)](https://github.com/cncjs/cncjs/wiki/Setup-Guide:-Raspberry-Pi-%7C-Install-Node.js-via-Package-Manager-*(Recommended)*#update-node-package-manager-npm)