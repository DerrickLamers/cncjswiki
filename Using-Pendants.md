## Introduction to Pendants

Pendants are CNCjs extensions that let you control CNCjs in other ways than the main CNCjs UI.  For example, there are pendants for minimal jogging on a small display (cncjs-pendant-tinyweb) and running jobs in a shop environment on a tablet computer (cncjs-shopfloor-tablet).

The fundamental way of accessing a pendant is by adding a suffix to the browser URL that points to CNCjs.  For example, if the main CNCjs URL is "http://localhost:8000/", you might access cncjs-shopfloor-tablet via "http://localhost:8000/tablet". Some specific scenarios are described below in the [Running Pendants](https://github.com/cncjs/cncjs/wiki/Using-Pendants#running-pendants) section.

_This page describes pendants that involve graphical controls.  There are some other "keyboard" pendants that do not involve a graphical user interface, but rather let you use the buttons on various kinds of keyboards and game controllers.  They work differently and are not described here._

## Installing a Pendant

The first step is to copy the pendant code to your computer.  You can do that either by "git cloning" the pendant code or by downloading and extracting a .zip file.  We will use cncjs-pendant-tinyweb as an example.  Browse to https://github.com/cncjs/cncjs-pendant-tinyweb .  Near the right side of the page there is a large green button labeled "Clone or Download".  Click on that to see options for getting the code.  If you are familiar with git, you can use use one of the git choices, which I will not describe further.  Otherwise, it is probably best to use the "Download ZIP" option.  That will download a .zip archive to your computer, which you can extract to a directory of your choice.  Let's assume that you extracted it to the directory  C:\Users\YOURNAME\Documents\cncjs-pendant-tinyweb  (the directory choice will obviously depend on which computer you are using).

The next step is to tell CNCjs where you put that pendant code.  CNCjs has a configuration file named .cncrc .  On Windows it is located in C:\Users\YOURNAME\.cncrc .  On Mac it is /Users/YOURNAME/.cncrc .  On Linux it is /home/YOURNAME/.cncrc .  You can edit that file with a text editor to add your pendant.  Typically .cncrc starts with lines like this:

```
{
    "state": {
        "checkForUpdates": true,
        "controller": {
            "exception": {
                "ignoreErrors": false
            }
        }
    },
    "secret": "$2a$10$LnW.uw2oK1f4.x31wTCSq.",
```

You need to add a "mountPoints" section.  It can go anywhere in the file, but you might as well put it at the beginning, just after the opening brace ({), as in this example

```
{
    "mountPoints": [
       {
           "route": "/tinyweb",
           "target": "C:/Users/wmb/Documents/cncjs-pendant-tinyweb/src"
       }
    ],
    "state": {
        "checkForUpdates": true,
        "controller": {
            "exception": {
                "ignoreErrors": false
            }
        }
    },
    "secret": "$2a$10$LnW.uw2oK1f4.x31wTCSq.",

```

* The "route" portion is the URL suffix you will use to access the pendant from the browser.  The name can be anything that you like.
* The "target" portion is the directory where you copied (git cloned, zip extracted, or whatever) the pendant code. You will need to edit the example above to reflect where you put the code. It is best to use '/' as a path separator, even on Windows machines where the official separator is '\'.
* Notice the "/src" suffix on the directory name.  If that is missing, it will not work.

You can have multiple pendants at once, like this

```
{
    "mountPoints": [
       {
           "route": "/tinyweb",
           "target": "C:/Users/wmb/Documents/cncjs-pendant-tinyweb/src"
       },
       {
           "route": "/tablet",
           "target": "C:/Users/wmb/Documents/cncjs-shopfloor-table/src"
       }
    ],
```
* The individual mounts go inside the [ ] brackets
* If you have more than one mount, they must be separated by commas, but the last one must NOT have a comma after it

## Running Pendants

To use a pendant you must browse to a URL that includes the pendant name at the end.  If the main CNCjs UI is at the URL "http://localhost:8000", the tinyweb pendant would be at "http://localhost:8000/tinyweb".  The suffix is not necessarily "tinyweb", but rather whatever name you used in the "route" line in .cncjs.

### Running Pendants from the CNCjs App

The "app" version of CNCjs does not use an external browser, but instead bundles a browser and everything else into a "click to run" application.  In that version, there is no URL bar, so you can't just add the right suffix to the URL.  Instead, the View dropdown menu has a "View in Browser" section at the bottom, with choices for all the pendants that you have installed.  If you click on one of those items, it will open an external browser with the URL set to the pendant.

### Running Pendants from Phones and Other Machines

The key to using a pendant from a machine other than the one running CNCjs is to find the right URL.  In the normal URL "http://localhost:8000", the "localhost" part refers to the IP address of the machine that is running the CNCjs server component. The "server" is the part of the code that talks directly to the CNC controller.  It runs on the computer that is physically connected - typically via a USB cable - to the CNC controller (Grbl, etc).  The CNCjs user interface, which runs inside a web browser, can run on that same computer or on any computer that can get to the "server" computer over the network.

To run the user interface - either the normal CNCjs UI or a pendant UI - on another computer, you need to know the IP address of the server computer.  Typically, in a local network, that IP address will be something like 192.168.1.15 .  The "192.168" part is pretty much the same on most home setups - it's a special prefix that means "this network is private to this location and you can't address it from the outside world".  The ".1" part is usually just that, i.e. ".1", but it could be different if have multiple routers on the same local network.  The ".15" part identifies a specific computer on the local network.  Every machine on the local net will have a different value there.  The catch is that the last component (e.g. ".15") is usually dynamically assigned and could possibly change if you reboot the computer.  So, once you find the IP address for your CNCjs server computer, it might not stay the same forever.

There are ways to force a given computer to have an address that will not change, but they differ from OS to OS.  Search the web for "assign static IP" to learn how to do it on different systems.

Once you have found the IP address of the CNCjs server computer, you can run a CNCjs user interface on any browser that is on that same network by entering a URL like "http://192.168.1.5:8000/tinyweb".  If you omit the /tinyweb suffix you will get the main CNCjs UI.  With the suffix you will get the selected pendant UI.

### Using the App with Browsers on Other Machines (like Phones)

The bundled "app" version of CNCjs works nicely when everything is running on one machine, but it is tricky - but not impossible - to access from outside machines.  There are several problems to solve:

* Instead of listening for any network connection that comes into the server machine, the app normally only listens for internal connections inside the same computer.  Instead of listening on the server computer's outside IP address - something like 192.168.1.15 - it listens on the special "myself" IP address 127.0.0.1.  You can't get to that from, say, a phone, because if you browse to http://127.0.0.1 on the phone, that means the phone itself. On any machine, 127.0.0.1 means "this machine".
* Instead of using the normal CNCjs port number 8000, the app chooses a random port number that can change every time.  This works within the app because it controls all components that run on the one machine, and can thus inform them of the special addressing.
* It is possible that a firewall - for example Windows Defender Firewall that comes bundled with Windows - might block external access to the CNCjs port, even after we set it to the standard 8000 one.

#### Forcing the App IP Address and Port (MacOS)

_I don't have a Mac anymore so I can't give detailed step by step instructions, and I can't test this; I got this information by piecing together stuff from the web.  Somebody with a Mac, please check this and fix it if necessary._

You need to use **AppleScript Editor** to make an executable script that uses "open" to run the command and add the --host and --port arguments.  The script command will look something like:

```
do shell script "open -a CNCjs --args -- --host 192.168.1.15 --port 8000"
```

Replace 192.168.1.15 with the IP address of your Mac.

#### Forcing the App IP Address and Port (Windows)

So how do we work around this?  You can tell the CNCjs app to use the main computer's normal IP address and the normal CNCjs port number.  Unfortunately, you can't do it in the .cncrc file; it has to be done on the command line that is used to start the app.  Getting to that command line is a bit of a dance.  Here is how to do it on Windows:

First, type CNCjs in the "Type here to search" Cortana bar at the bottom of the screen.  That should find the CNCjs app. 
 Choose Open File Location:

![image](https://user-images.githubusercontent.com/4861133/76241521-339dd280-61d9-11ea-9eeb-f35866c6a6c2.png)

That will take you to an Explorer window with the CNCjs shortcut highlighted. Right-click on that and choose Properties:

![image](https://user-images.githubusercontent.com/4861133/76240813-f7b63d80-61d7-11ea-987a-277d0716e80e.png)

That, in turn, will bring up a Properties panel:

![CNCjs-shortcut](https://user-images.githubusercontent.com/4861133/76240477-6777f880-61d7-11ea-9a71-f0e9bffc3257.png)

You will need to change the value of the Target field.  It will normally contain something like

`C:\Users\wmb\AppData\Local\Programs\cncjs-app\CNCjs.exe`

You need to add stuff to the end, which will require clicking in the box and using the arrow keys to scroll to the end, then typing the new stuff:

`C:\Users\wmb\AppData\Local\Programs\cncjs-app\CNCjs.exe --host 192.168.2.12 --port 8000`

In the host field, you need the actual IP address of your PC.  Ask the internet to teach you how to find it if you don't already know.  Unfortunately, as mentioned above, it might not stay the same forever unless you have assigned a static IP address.  The --port field should be set to 8000.

### Getting Through the Windows Firewall

(If you think doing this is tedious (it is), you should try writing these instructions and getting all the screenshots.)

#### Firewall Off - Easy, but Insecure
The easy, and insecure, way is to turn off the Windows Firewall for private networks:

First type "Firewall" in the Cortana "Type here to search" box.  That should bring up a Windows Security window.

![image](https://user-images.githubusercontent.com/4861133/76243184-030b6800-61dc-11ea-84fc-85e896d0f2dc.png)

I assume that you are on a private network.  You really should not turn off the firewall on a public one.  Click on Private Network to get to this screen:

![image](https://user-images.githubusercontent.com/4861133/76244247-c80a3400-61dd-11ea-8e92-f3fbe44d9ad6.png)

Click on the On button to turn it off, then allow the changes when the warning pops up.

Now you should be able to access CNCjs from another device, using a URL like http://192.168.1.15:8000, or a pendant with a URL like http://192.168.1.15:8000/tinyweb .

#### Special Firewall Rule - Harder, More Secure

The right way to allow CNCjs access from another machine is to create a special firewall rule.  Start by typing "firewall" in the Cortana "Type here to search" box, bringing up this screen:

![image](https://user-images.githubusercontent.com/4861133/76244041-6944ba80-61dd-11ea-94a8-4e268c21bde0.png)

Click On Advanced Settings for this screen:

![image](https://user-images.githubusercontent.com/4861133/76244823-e7ee2780-61de-11ea-9f91-0dede902ecf6.png)

Click on Inbound Rules to get this:

![image](https://user-images.githubusercontent.com/4861133/76244955-284da580-61df-11ea-91cc-5cef1c11910a.png)

Click on New Rule... to get this:

![image](https://user-images.githubusercontent.com/4861133/76245138-719df500-61df-11ea-927b-a82b218e9079.png)

Choose Port and Next

![image](https://user-images.githubusercontent.com/4861133/76245257-a447ed80-61df-11ea-8fdb-3e3008efb3ee.png)

Choose TCP, enter port 8000, and Next

![image](https://user-images.githubusercontent.com/4861133/76245331-c5104300-61df-11ea-8813-6f693d84209a.png)

Choose Allow

![image](https://user-images.githubusercontent.com/4861133/76245405-ea04b600-61df-11ea-9a82-88070b6c8740.png)

Choose Domain and Private.  It is not safe to allow access on CNCjs on public networks (although you could set up CNCjs to require a user login - but that is a separate topic).

![image](https://user-images.githubusercontent.com/4861133/76245493-16203700-61e0-11ea-9be0-ab0c40526bbc.png)

Enter the name for this rule and a description, and hit Finish

![image](https://user-images.githubusercontent.com/4861133/76245721-76af7400-61e0-11ea-99bc-4c8f887b0f38.png)

When you succeed there will be a new CNCjs rule that allows incoming connections to the CNCjs server.

