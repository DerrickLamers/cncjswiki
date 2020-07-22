This is the official way to install CNCjs on a Raspberry Pi.  There are some alternative ways, but this is the one you should use unless you have a good reason - and are familiar enough with Linux and the node.js ecosystem to understand the ins and outs of the various tools.  Using this approach will make it easier to get help on the Facebook CNCjs User's Group.

### Basics of Terminal Commands

The first step is to open a terminal (console) window.  If you are using the graphical environment, do that by clicking on the black square icon at the upper left.  If you are not using the graphical environment, you are probably already in a terminal environment.  A terminal prompt looks like **pi@HOSTNAME:~ $**.  "pi" is the user name - on a Raspberry Pi, the default username is "pi", and the instructions below assume that you have not changed that.  "HOSTNAME" is the name of the machine itself - if you have multiple Raspberry Pis, they will probably have different names.  The part after the ":" is the directory name - "\~" is shorthand for "the current user's home directory".  If you have changed to a different directory, that field with show where you are.

**In the following discussion, when it says to enter something at the terminal prompt, you can type them manually, but it is much better to copy and paste from these instructions to avoid mistakes.**

### Install Node.js via Package Manager

Enter these commands, one at a time, at the terminal prompt.

```
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt install -y nodejs build-essential npm
sudo npm install -g npm@latest
```

Ignore warnings from the npm install step.  It is just being annoying.

```
sudo npm install -g cncjs@latest --unsafe-perm
```
This will take awhile and generate quite a lot of output as shown [here](https://github.com/cncjs/cncjs/wiki/Setup-Guide:-Raspberry-Pi-%7C-Example-Output-from-Installation).  Note that WARN lines are not errors, so do not worry about them.  If this step appears to fail - by generating ERROR lines - you can look [here](https://github.com/cncjs/cncjs/wiki/Setup-Guide:-Raspberry-Pi-%7C-Installation-Problems-and-Solutions) for possible solutions.

### Test the Installation

At this point you should run a quick test to see if things are working.  Enter this at the terminal prompt:

```
cncjs
```
Something like this should be displayed in the terminal:
```
2019-10-04T22:45:16.701Z - info init Loading configuration from "/home/pi/.cncrc"
2019-10-04T22:45:17.931Z - info init Starting the server at http://127.0.1.1:8000
```
That means the CNCjs server is running, ready to accept connections from a browser.  Start the Chromium browser on your Pi (other browsers than Chromium will probably work too) and enter the URL "http://127.0.1.1:8000" (i.e. the URL shown in the "Starting the server" message above).  The browser should say "Loading" and after a few seconds the CNCjs user interface should appear.

To stop the CNCjs server, type ^C (Ctrl-C) in the terminal.

If your Pi is headless (no graphics display), you can connect from a browser on a different machine on the same network as the Pi.  To find the Pi's external IP address, you can use the command `hostname -I`.  Let's assume that the Pi's external IP address is "192.168.2.23" - then you would browse to "192.168.2.23:8000" from that other machine.  (If you had to stop the server with ^C in order to run the hostname command, you will need to restart it with the `cncjs` command.)

* Possible Install Problems & Solutions:

  * #### Serialport version problem

   You might see an error message like this:
   ```
   Error: Error: The module '/usr/lib/node_modules/cncjs/node_modules/serialport/build/Release/serialport.node'
   was compiled against a different Node.js version using NODE_MODULE_VERSION 57. This version of Node.js
   requires NODE_MODULE_VERSION 64. Please try re-compiling or re-installing the module (for instance,
   using `npm rebuild` or `npm install`).
   ```
   To fix that, enter these commands at the terminal prompt:
   ```
   cd /usr/lib/node_modules/cncjs/node_modules
   sudo npm rebuild --update-binary --unsafe-perm 
   cd ~   
   ```
   There should be many lines of output as it recompiles the serialport code ([sample output](https://github.com/cncjs/cncjs/wiki/Setup-Guide:-Raspberry-Pi-%7C-Output-from-npm-rebuild-serialport)).  When it finishes, you can retry the `cncjs` command and connect from a browser.

After you get a successful test, kill the CNCjs server by typing ^C (Ctrl-C) and proceed to the next step, whose purpose is to make the CNCjs server start automatically every time you reboot the Pi.  Note that starting the server *does not* start the user interface in a browser.  Autostarting the browser requires an additional step.


### Autostarting the server

To make the CNCjs server start automatically, enter this verbatim at the terminal prompt:

```
((crontab -l || true) | grep -v cncjs; echo "@reboot $(which cncjs) >>$HOME/cncjs.log 2>&1") | crontab -
```
If it says "no crontab for pi", don't worry about it.

That looks pretty cryptic, so I will break it down in cause you are interested.  `(crontab -l || true)` lists the current contents of the user's crontab file (the file that controls automatic execution of programs) - but if there is no crontab file, it just continues.  `| grep -v cncjs` takes the output of the previous command and removes any existing lines that contain the word 'cncjs' (necessary in case you re-execute the command; you don't want to start CNCjs twice).  `echo "SOMETHING"` emits a line containing SOMETHING.  '( A ; B )` appends the output of the command B to the end of the command A, so the echo'ed line gets added to the end of the existing crontab file minus any previous cncjs lines. `| crontab -` takes what comes before and puts it back in the crontab file.

SOMETHING in this case is `@reboot $(which cncjs) >>$HOME/cncjs.log 2>&1` .  `@reboot` is the crontab time specification for "run the following command once every time the computer reboots".  `$(which cncjs)` means "find where the cncjs executable command is located and insert the full pathname in place of the `$(which cncjs)`.  `>>$HOME/cncjs.log` means "append any normal (non-error) output of the command to the file cncjs.log in the user's home directory".  `2>&1` means "also send error output to the same place that normal output is going".

If you need to add arguments to the cncjs command, the extra arguments go between the `$(which cncjs)` and the `>>$HOME/cncjs.log`.

If you want to turn off auto-start, you can enter this at the terminal prompt:

```
crontab -l | grep -v cncjs | crontab -
```

and to turn it back on you can repeat the earlier recipe.  Alternatively you can enter `crontab -e` and then use the text editor that pops up to manually edit the file and delete or comment-out (with #) the `@reboot ...` line.


### Autostarting the browser

Starting the server as above does not start a user interface; to get the user interface you need to run a browser and point it at localhost:8000 as per the earlier instructions in the **Test the Installation** section of this page.  The browser can, but does not have to, run on the Pi with the server.  Some people use the Pi only for the server, running the browser on a different computer, or a tablet, or even on a smartphone.

Running the browser on the Pi works best with a faster Pi like a Pi3 or Pi4.  The CNCjs user interface performance on lower-end Pis is marginal at best.  Running only the cncjs server works well on any Pi, even a Pi Zero.

To make the Chromium browser start automatically, and display the CNCjs user interface, enter this at the terminal prompt:

```
cd /home/pi

cat >startCNCjsUI <<EOF
#!/bin/bash

# Prevent the screen from turning off
xset s off
xset -dpms

# Show the user why it is taking so long
zenity --info --no-wrap --timeout 240 --width=400 --height=400 --text="Waiting for CNCjs server to start" &

# Wait until the CNCjs server becomes responsive before starting the browser
until $(curl --output /dev/null --silent --head --fail http://localhost:8000); do
  sleep 1
done

# Finally, start the browser, pointing to the CNCjs server
# --kiosk makes the browser occupy the entire screen.
# If you want to kill the full-screen browser, use ALT-F4
# If you omit --kiosk, the browser will start in a normal window
chromium-browser --kiosk http://localhost:8000
EOF

chmod a+x startCNCjsUI

mkdir -p .config/lxsession/LXDE-pi

cat >.config/lxsession/LXDE-pi/autostart <<EOF
@lxpanel --profile LXDE-pi
@pcmanfm --desktop --profile LXDE-pi
/home/pi/startCNCjsUI
EOF
```
That does two things
* It creates an executable script named _startCNCjsUI_ that waits for the cncJS server to finish starting, then starts a full-screen browser to talk to the server.
* It creates an autostart file _.config/lxsession/LXDE-pi/autostart_ to override the system-supplied startup file for the windowing environment.  The new autostart file invokes the _startCNCjsUI_ script.

The net result is that the CNCjs user interface will start automatically and take over the screen, as soon as the system is ready.
### Reboot to test
```sudo reboot```

If all goes well, the Pi should reboot, then start the windowing environment.  A message should appear saying that it is waiting for the CNCjs server to start.  After a few seconds (possibly almost instantly on a newer, faster Pi), the browser should start, connect to CNCjs, and run the CNCjs user interface.