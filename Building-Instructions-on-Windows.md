# Installation Requirements

Follow the guidelines below for building a desktop application for Windows.

## Git For Windows

https://gitforwindows.org/

Git for Windows provides a BASH emulation used to run Git and standard unix commands from the command line. It is the essential part of the build process.

## Python 2.7

https://www.python.org/downloads/release/python-2716/

Be sure to check **Add python.exe to Path**

![image](https://user-images.githubusercontent.com/447801/56357352-1ef5cc00-620e-11e9-9d8f-cf9431a7453f.png)

## Node.js 10

https://nodejs.org/en/download/

## Windows Build Tools

Install Windows Build Tools under **PowerShell**. You need to right-click on **PowerShell** and run "as Administrator".

```sh
npm install --global --production windows-build-tools
npm config set msvs_version 2017
```

![image](https://user-images.githubusercontent.com/447801/56358007-e8b94c00-620f-11e9-99e5-e8c295350d76.png)

![image](https://user-images.githubusercontent.com/447801/56357893-90824a00-620f-11e9-8140-4788523f5e23.png)

# Cloning the Repository

Launch **Git Bash** to clone the repository from the command line, do not use **Windows Command Prompt** or **Windows PowerShell**.

```sh
$ git clone https://github.com/cncjs/cncjs.git
$ cd cncjs
$ npm i
```

# Building the Code

```sh
$ npm run prepare
$ npm run build:win-x64
```

You will see the log output when the build is successful:
```
> cncjs@1.9.18 build:win-x64 D:\GitHub\cncjs
> bash -c 'scripts/electron-builder.sh --win --x64'

/d/GitHub/cncjs/dist/cnc /d/GitHub/cncjs
Cleaning up "/d/GitHub/cncjs/dist/cnc/node_modules"
Installing packages...

> serialport@6.2.2 install D:\GitHub\cncjs\dist\cnc\node_modules\serialport
> prebuild-install || node-gyp rebuild

added 529 packages from 292 contributors and audited 1395 packages in 40.654s
found 9 vulnerabilities (3 low, 5 moderate, 1 high)
  run `npm audit fix` to fix them, or `npm audit` for details
audited 1395 packages in 2.994s
found 9 vulnerabilities (3 low, 5 moderate, 1 high)
  run `npm audit fix` to fix them, or `npm audit` for details
/d/GitHub/cncjs
Rebuild native modules using electron
v4.1.4

> cncjs@1.9.18 electron-rebuild D:\GitHub\cncjs
> electron-rebuild "--version=" "v4.1.4" "--module-dir=dist/cnc" "--which-module=serialport"

- Searching dependency tree
√ Rebuild Complete

> cncjs@1.9.18 electron-builder D:\GitHub\cncjs
> build "--win" "--x64"

Configuring yargs through package.json is deprecated and will be removed in the next major release, please use the JS API instead.
Configuring yargs through package.json is deprecated and will be removed in the next major release, please use the JS API instead.
  • electron-builder version=20.39.0
  • loaded configuration file=package.json ("build" field)
  • rebuilding native production dependencies platform=win32 arch=x64
  • packaging       platform=win32 arch=x64 electron=4.1.4 appOutDir=output\win-unpacked
  • asar using is disabled — it is strongly not recommended solution=enable asar and use asarUnpack to unpack files that must be externally available
  • downloading               parts=1 size=5.6 MB url=https://github.com/electron-userland/electron-builder-binaries/releases/download/winCodeSign-2.4.0/winCodeSign-2.4.0.7z
  • downloaded                duration=4.564s url=https://github.com/electron-userland/electron-builder-binaries/releases/download/winCodeSign-2.4.0/winCodeSign-2.4.0.7z
  • building        target=nsis file=output\CNCjs Setup 1.9.18.exe archs=x64 oneClick=true perMachine=false
  • downloading               parts=1 size=1.4 MB url=https://github.com/electron-userland/electron-builder-binaries/releases/download/nsis-3.0.3.2/nsis-3.0.3.2.7z
  • downloaded                duration=3.861s url=https://github.com/electron-userland/electron-builder-binaries/releases/download/nsis-3.0.3.2/nsis-3.0.3.2.7z
  • downloading               parts=1 size=1.0 MB url=https://github.com/electron-userland/electron-builder-binaries/releases/download/nsis-resources-3.3.0/nsis-resources-3.3.0.7z
  • downloaded                duration=4.651s url=https://github.com/electron-userland/electron-builder-binaries/releases/download/nsis-resources-3.3.0/nsis-resources-3.3.0.7z
  • building block map blockMapFile=output\CNCjs Setup 1.9.18.exe.blockmap

Cheton Wu@TW-CHETON-WIN10 MINGW64 /d/GitHub/cncjs (master)
$ ls -al output/
total 51853
drwxr-xr-x 1 Cheton Wu 197121        0 Apr 18 19:31  ./
drwxr-xr-x 1 Cheton Wu 197121        0 Apr 18 19:29  ../
-rwxr-xr-x 1 Cheton Wu 197121 53014176 Apr 18 19:31 'CNCjs Setup 1.9.18.exe'*
-rw-r--r-- 1 Cheton Wu 197121    56336 Apr 18 19:31 'CNCjs Setup 1.9.18.exe.blockmap'
-rw-r--r-- 1 Cheton Wu 197121      349 Apr 18 19:31  latest.yml
drwxr-xr-x 1 Cheton Wu 197121        0 Apr 18 19:29  win-unpacked/
```

![image](https://user-images.githubusercontent.com/447801/56358242-ae03e380-6210-11e9-8afa-2bcc84bcdfae.png)

Now you can open Windows Explorer to run the installer:

![image](https://user-images.githubusercontent.com/447801/56358414-1c48a600-6211-11e9-9ac9-1167a8944e9d.png)
