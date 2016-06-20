## Index

* [Webcam Streaming with Raspberry Pi](https://github.com/cheton/cnc/wiki/FAQ/#webcam-streaming-with-raspberry-pi)
* [Restream RTSP to M-JPEG](https://github.com/cheton/cnc/wiki/FAQ/#restream-rtsp-to-m-jpeg)
* [Connect to an Arduino using WiFi](https://github.com/cheton/cnc/wiki/FAQ#connect-to-an-arduino-using-wifi)
* [Install Native Addons with Node.js v4](https://github.com/cheton/cnc/wiki/FAQ#install-native-addons-with-nodejs-v4)
* [Install Serialport on OS X El Capitan](https://github.com/cheton/cnc/wiki/FAQ/#install-serialport-on-os-x-el-capitan)

## Webcam Streaming with Raspberry Pi

Checkout [mjpg-streamer](https://github.com/jacksonliam/mjpg-streamer) to learn how to [stream JPEG data from the input_raspicam plugin via HTTP](https://github.com/jacksonliam/mjpg-streamer/blob/master/mjpg-streamer-experimental/plugins/output_http/README.md), and follow the steps in this [article](http://www.howtoembed.com/projects/raspberry-pi/78-pieye-webcam-streaming-in-m-jpg-format-with-raspberry-pi):

1. Make sure you have an updated version of <i>Raspberry Pi</i>'s OS.
2. Install `libv4l-0` package, available in Raspbian: `sudo aptitude install libv4l-0`.
3. Connect the web camera to USB. The web camera must be Linux compatible; to check this, make sure `/dev/video0` file is available on <i>Raspberry Pi</i>, else the camera does not have a Linux driver or required extra configuration to work.
4. Download [mjpg-streamer-rpi.tar.gz](http://www.howtoembed.com/projects/raspberry-pi/78-pieye-webcam-streaming-in-m-jpg-format-with-raspberry-pi#rpi-mjpg-streamer) archive on <i>Raspberry Pi</i> and extract it. Destination folder is not relevant. You don't need root access if you are using the default pi user. Go to `mjpg-streamer` folder, where you extracted the `tar.gz` file.
5. Open `mjpg-streamer.sh` file; this is a simple bash script to control the mini-webserver. The header contains some writable parameters, as refresh rate or resolution. The default settings should work in most situations.
6. Start the server with `./mjpg-streamer.sh` start command in the current folder.
7. Run your prefered web browser and go to <b>http://raspberrypi:8080/?action=stream</b> (where raspberrypi is it's IP address). You should see the image from the webcam. Current version has some issues with Chrome, just use Firefox if the image is not refreshed.
8. If the system doesn't work, see the `mjpg-streamer.log` file for debug info.

See below required commands for not-Linux geeks:
```bash
$ sudo aptitude install libv4l-0
$ ls /dev/video0
$ wget http://www.bobtech.ro/get?download=36:mjpg-streamer-rpi
$ mv get\?download\=36\:mjpg-streamer-rpi mjpg-streamer-rpi.tar.gz
$ tar -zxvf mjpg-streamer-rpi.tar.gz
$ cd mjpg-streamer
$ sudo nano mjpg-streamer.sh
$ ./mjpg-streamer.sh start
```

Once you have finished setup, input the URL in the webcam widget to play the M-JPEG stream:
```bash
# Replace raspberrypi with your IP address
http://raspberrypi:8080/?action=stream
```

#### Change Video Parameters
To change video parameters you have to install <b>v4l2-ctl</b>:
```bash
$ sudo apt-get install v4l-utils
$ v4l2-ctl --list-ctrls
$ v4l2-ctl --set-ctrl brightness=200
$ v4l2-ctl --set-ctrl saturation=32
```

#### Download
http://www.howtoembed.com/get?download=36:mjpg-streamer-rpi

## Restream RTSP to M-JPEG

**<i>FFmpeg is available for Windows and Linux, but FFserver is available only on Linux.</i>**

Sample `ffserver.conf` file:
```
HTTPPort 8090

<Feed webcam.ffm>
  File /tmp/webcam.ffm
  FileMaxSize 50M
</Feed>

<Stream webcam.mjpg>
  Feed webcam.ffm
  Format mpjpeg
  VideoBufferSize 8000
  VideoCodec mjpeg
  VideoFrameRate 24
  VideoSize 640x480
  NoAudio
</Stream>
```

After that started server with command:
```
ffserver -d -f ffserver.conf
```

and run streaming with command:
```
ffmpeg -i "rtsp://<ip-camera>/" http://localhost:8090/webcam.ffm
```

Now you can input the URL in the webcam widget to play the M-JPEG stream:
```
http://localhost:8090/webcam.mjpg
```

## Connect to an Arduino using WiFi
![WaveShare WIFI-LPT100 / WIFI400](https://raw.githubusercontent.com/cheton/cnc/master/media/WS_WIFI-LPT100_WIFI400.png)

These articles might be useful if you want to connect to Arduino using WiFi: 
* [WiFi your nodebot](https://gist.github.com/ajfisher/1fdbcbbf96b7f2ba73cd)
* [Arduino Wifi With Hi Flying HF-LPT100/USR WIFI232-T](http://2xod.com/articles/Arduino_Wifi_With_Hi_Flying_HF-LPT100_or_USR-WIFI232/)

### Module Setup (w/ Breakout Board)
![WIFI400](http://cheton.github.io/jsdc2015/images/usr-wifi232/HF-LPT100-Breakout-WIFI400-Front-sm.jpg)

| WiFi232      | Arduino    |
|--------------|------------|
| Pin 1 (GND)  | GND        |
| Pin 2 (3.3V) | 3V3 (3.3V) |
| Pin 5 (RX)   | Pin 1 (TX) |
| Pin 6 (TX)   | Pin 0 (RX) |

### Johnny-Five Setup
Node.js &ndash; Pseudo Terminal (~/dev/ttyV0) &ndash; TCP Socket (10.0.1.12:8899)

1. Use `socat` to fake a serial terminal:
```bash
socat -d -d pty,nonblock,link=$HOME/dev/ttyV0 tcp:10.0.1.12:8899
```
2. Execute it like this:
```bash
node blink.js ~/dev/ttyV0
```

## Install Native Addons with Node.js v4
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

## Install Serialport on OS X El Capitan
If you're running with OS X El Capitan (version: 10.11), and the [installation](https://github.com/cheton/cnc#installation) fails with error `ld: library not found for -lgcc_s.10.5`, check out the following ways:
- Check out [Installing the Xcode Command Line Tools](https://developer.xamarin.com/guides/testcloud/calabash/configuring/osx/install-xcode-command-line-tools/).
- If you do not have Xcode installed, just install Xcode 7 from App Store.
- There is an issue with Mac OS X 10.11 and Xcode 6. If your Xcode version is 6.x, you need to upgrade it to Xcode 7, or use this as temporary fix:

```bash
cd /usr/local/lib
sudo ln -s ../../lib/libSystem.B.dylib libgcc_s.10.5.dylib
```