## Install native addons with Node.js v4
Source: https://github.com/fivdi/onoff/wiki/Node.js-v4-and-native-addons

### Installing gcc/g++ 4.8 on Raspbian Wheezy for the Raspberry Pi
Run `apt-get update` to update the system's package list:
```bash
sudo apt-get update
```

Install gcc/g++ 4.8 as below:
```bash
sudo apt-get install gcc-4.8 g++-4.8
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.6 20
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.8 50
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.6 20
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.8 50
```
Check to see which versions of gcc/g++ are installed:
```bash
pi@raspberrypi ~ $ gcc --version
gcc (Raspbian 4.8.2-21~rpi3rpi1) 4.8.2
Copyright (C) 2013 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

pi@raspberrypi ~ $ g++ --version
g++ (Raspbian 4.8.2-21~rpi3rpi1) 4.8.2
Copyright (C) 2013 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```

It should now be able to install native addons with io.js v3 or Node.js v4 or higher.

If required, use the following commands to switch between the different version gcc/g++, like so:
```bash
sudo update-alternatives --config gcc
sudo update-alternatives --config g++
```

## Install serialport with Mac OS X 10.11
If your Mac OS X is El Capitan (version: 10.11), and the [installation](https://github.com/cheton/cnc.js#installation) fails with error `ld: library not found for -lgcc_s.10.5`, check out the following ways:
- Check out [Installing the Xcode Command Line Tools](https://developer.xamarin.com/guides/testcloud/calabash/configuring/osx/install-xcode-command-line-tools/).
- If you do not have Xcode installed, just install Xcode 7 from App Store.
- There is an issue with Mac OS X 10.11 and Xcode 6. If your Xcode version is 6.x, you need to upgrade it to Xcode 7, or use this as temporary fix:

```bash
cd /usr/local/lib
sudo ln -s ../../lib/libSystem.B.dylib libgcc_s.10.5.dylib
```

## How do I connect to an Arduino using WiFi? 
![WaveShare WIFI-LPT100 / WIFI400](https://raw.githubusercontent.com/cheton/cnc.js/master/media/WS_WIFI-LPT100_WIFI400.png)

These articles might be useful if you want to connect to Arduino using WiFi: 
* [WiFi your nodebot](https://gist.github.com/ajfisher/1fdbcbbf96b7f2ba73cd)
* [Arduino Wifi With Hi Flying HF-LPT100/USR WIFI232-T](http://2xod.com/articles/Arduino_Wifi_With_Hi_Flying_HF-LPT100_or_USR-WIFI232/)

## Module Setup (w/ Breakout Board)
![WIFI400](http://cheton.github.io/jsdc2015/images/usr-wifi232/HF-LPT100-Breakout-WIFI400-Front-sm.jpg)

| WiFi232      | Arduino    |
|--------------|------------|
| Pin 1 (GND)  | GND        |
| Pin 2 (3.3V) | 3V3 (3.3V) |
| Pin 5 (RX)   | Pin 1 (TX) |
| Pin 6 (TX)   | Pin 0 (RX) |


## Johnny-Five Setup
Node.js &ndash; Pseudo Terminal (~/dev/ttyV0) &ndash; TCP Socket (10.0.1.12:8899)

1. Use `socat` to fake a serial terminal:
```bash
socat -d -d pty,nonblock,link=$HOME/dev/ttyV0 tcp:10.0.1.12:8899
```
2. Execute it like this:
```bash
node blink.js ~/dev/ttyV0
```