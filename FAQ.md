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