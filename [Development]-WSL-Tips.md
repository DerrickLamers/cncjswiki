# Running CNCjs under WSL

[WSL (Wndows Subsystem for Linux)](https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux) lets you run Linux programs under Windows 10 with somewhat less hassle than virtualization alternatives like VirtualBox, Docker, etc.  You still have to install a Linux distro, but afterwards WSL starts faster than a VM and is somewhat better integrated with the Windows filesystem.  It's not perfect, but many things work rather well, and it is getting better, as Microsoft appears to be committed to improving it.  Microsoft responded almost instantly to a bug report that I filed, and fixed it quickly.

CNCjs can be made to run under WSL.  Compilation is identical to CNCjs compilation under Linux.  Just clone the git repo and say `npm install`, and the build process succeeds.

The difficulty is accessing serial ports.  WSL exposes Windows COM ports under the standard Linux names; for example WSL presents COM3 as /dev/ttyS3.  But there are two problems:
1. In WSL, the tty ports are owned by root,root with permissions crw-r-----.  You can change that ownership, but when you restart WSL they revert back to the original ownership and permissions.  Apparently Microsoft is going to fix this (https://github.com/Microsoft/WSL/issues/3042) but the fix hasn't been rolled out in the Windows build I use.  The workaround is to run cncjs as root, e.g.  `sudo bin/cncjs`
2. The node.js **serialport** package doesn't list the serial ports under WSL.  **serialport**, when running under WSL, thinks it is on an ordinary Linux and uses Linux's **udev** facility to discover serial ports.  WSL's rudimentary implementation of udev does not include serial ports.  To work around this, I modified serialport as follows.
In node_modules/@serialport/bindings/lib/ I created a new file wsl-list.js with the following contents:
```
function listLinux() {
    const ports = [
        { comName: "/dev/ttyS3"},
        { comName: "/dev/ttyS4"}
    return Promise.resolve(ports);
}
module.exports = listLinux
```
For simplicity, I hardcoded only the serial ports that connect to my test controllers.

In node_modules/@serialport/bindings/linux.js, I changed the line
```
const linuxList = require('./linux-list')
```
to
```
const linuxList = require('./wsl-list')
```
You can, of course, compile CNCjs for Windows without WSL, but, apart from the serialport thing, WSL is a bit less trouble if you already have it set up.
There is no need to rebuild cncjs.  All you have to do is kill the cncjs process and restart it.

## Alternative to modifying cncjs

An alternative that does not require modifying cncjs mods is to create an executable shell script named **udevadm** to override the system version. That script would contain, for example:
```
#! /bin/sh
echo P: dummy
echo N: ttyS3
echo E: DEVNAME=/dev/ttyS3
echo P: dummy
echo N: ttyS4
echo E: DEVNAME=/dev/ttyS4
```
Since cncjs must be run as root (per problem 1. above), the script must be in the super user search path before the real version of udevadm.  In my WSL setup, the super user search path is `/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin` and the real udevadm is found in /sbin, so you could put your udevadm in, for example, /usr/local/sbin.  The downside of this approach is that any other command that uses udevadm will probably fail.  I think that `apt-get upgrade` could be a problem area, per [this WSL FAQ](https://docs.microsoft.com/en-us/windows/wsl/faq) .  If you run into problems, you could temporarily rename your spoofed version of udevadm .  Or make a fancier script that bounces to the system udevadm unless it is called just the way that cncjs calls it (`udevadm info -e`). Or just use the wsl-list.js hack instead.

