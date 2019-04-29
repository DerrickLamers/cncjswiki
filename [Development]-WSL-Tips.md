# Running CNCjs under WSL

[WSL (Wndows Subsystem for Linux)](https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux)lets you run Linux programs under Windows 10 with somewhat less hassle than virtualization alternatives like VirtualBox, Docker, etc.  You still have to install a Linux distro, but then WSL starts faster than a virtualized solution and is somewhat better integrated into Windows.  It's not perfect, but many things work reasonably well, and it is getting better, as Microsoft appears to be committed to improving it.

CNCjs can be made to run under WSL.  Compilation is quite straighforward, essentially identical to CNCjs compilation under Linux.  Basically you just say `npm install` and the build process succeeds.

The main problem is accessing serial ports.  WSL exposes Windows COM ports under the standard Linux names; for example WSL presents COM3 as /dev/ttyS3.  But there are two problems:
1. In WSL, the tty ports are owned by root,root with permissions crw-r-----.  You can change that ownership, but when you restart WSL they revert back to the original ownership and permissions.  Apparently Microsoft is going to fix this (https://github.com/Microsoft/WSL/issues/3042) but the fix hasn't been rolled out in the Windows build I use.  The workaround is to run cncjs as root, e.g.  `sudo bin/cncjs`
2. The node.js **serialport** package doesn't list the serial ports under WSL.  **serialport**, when running under WSL, thinks it is on an ordinary Linux and uses Linux's **udev** facility to discover serial ports.  WSL's rudimentary implementation of udev does not include serial ports.  To work around this, I modified serialport as follows.
In node_modules/@serialport/bindings/lib/ I created a new file wsl-list.js with the following contents:

`function listLinux() {`
    `const ports = []`

    `ports.push({ comName: "/dev/ttyS3"} );`
    `ports.push({ comName: "/dev/ttyS4"} );`

    `return Promise.resolve(ports);`
`}`
`module.exports = listLinux`

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