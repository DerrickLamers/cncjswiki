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
