My setup has a Raspberry Pi Zero (running Raspbian Stretch) connected to an Arduino Due running g2core.

When the Due powers up, it usually starts in bootloader mode, instead of presenting the TinyG serial port on the USB interface.  To connect with CNCjs, it is necessary to reset the Due so the USB serial port will speak the TinyG protocol instead of the bootloader protocol.

To solve this problem, I connected a wire from the Pi's GPIO4 to the Due's reset pin.  Running the following "resetDue" script on the Pi will then reset the Due:

```
#!/bin/sh
# Drive the TinyG controller reset line low then high.  This ensures that
# it comes up in the right USB mode
echo 4 >/sys/class/gpio/export 2>/dev/null
sleep 5
echo out >/sys/class/gpio/gpio4/direction
echo 0 >/sys/class/gpio/gpio4/value
sleep 3
echo 1 >/sys/class/gpio/gpio4/value
```
For that script to work as user "pi", "pi" must be a member of the "gpio" group, e.g.

`usermod -G gpio
`

I run that script prior to starting CNCjs, via the following "startCNC" script:

```
#!/bin/sh
/home/pi/resetDue
env PATH=$PATH:/usr/local/bin /home/pi/GitHub/cncjs/bin/cnc -H 192.168.2.100 -p
8000 >> $HOME/cncjs.log 2>
&1
```

startCNC is executed from the "pi" user's crontab via the following line:

`@reboot /home/pi/startCNC
`

The reset script will fail if it is run too early, possibly due to problems with the ordering of different startup scripts.  To solve that problem, I had to make crontab wait until the network is up.  I did that by adding this line to the [Unit] section of /lib/systemd/system/cron.service:

`After=rc-local.service
`

That depends on the fact that rc.local runs after networking.  It might have worked to say

`After=network.target
`

but I did not try that.

There is never a dull moment trying to make something work under Linux.

