Getting the full cncjs stack running on a Raspberry Pi can be quite challenging.  Herein is a rundown of numerous hurdles.

The cncjs server component works quite well on any Raspberry Pi, from the Pi Zero up to the Pi3, but running the cncjs user interface on a Pi is a different story.

### Background

cncjs consists of two separate components, the server and the user interface.  The cncjs server is a program that communicates at the low level - via a serial port - to a external machine controller board, and at the high level - via a network connection - to a web browser that displays the user interface.

The machine controller board runs Grbl, TinyG/g2core, or Smoothieware firmware; its job is to receive GCode language commands from a serial port, translating them into the signal sequences needed to run stepper motors and other machine components.  The computer that runs the cncjs server must be connected to the machine controller board via a serial interface - typically either a hardware "UART" port or a USB-to-serial adapter, but it could also be a wireless serial interface.

The cncjs user interface is a Javascript program that runs inside a standard web browser like Chrome, Firefox, or Safari.  That web browser can run on any computer that is networked - via LAN, WiFi, or even across the Internet - to the computer running the cncjs server program.  The same computer can run both the browser and the server, or they can be on different computers.  A single cncjs server program can even connect to multiple browsers, so you could, for example, have instances of the cncjs user interface for a single CNC machine running at the same time on a laptop, a smart phone, and a tablet.

Even though the cncjs user interface program runs on a browser that might be physically separated from the cncjs server computer, that program is actually stored on the server computer.  When the browser connects to the cncjs server, the server delivers the program to the browser over the network, in the same way that a web server delivers a web page to a browser halfway around the world.

### Running on Raspberry Pi

Raspberry Pi computers are limited compared to modern PCs and even to smartphones.  Pis have on the order of 1/10 the memory, 1/10 the CPU speed, 1/100 the mass storage, and significantly less graphics horsepower and memory bandwidth, than modern PCs.  Of course their cost is correspondingly less.  The "high end" Pi3 is more capable than its lower-cost siblings, but is still quite weak compared to a PC.

The area where this affects cncjs the most is the user interface.  A Pi, even the smallest, does a decent job at running the cncjs server.  The server code is written in Javascript using the NodeJS framework.  That is not the most efficient platform compared to, say, C++ code, but even so a Pi does a reasonable job of running that server code.  The server itself does not use a huge amount of memory nor does it perform graphics operations.  It mostly just shuttles small chunks of information between a network connections and a serial port.  The user interface, on the other hand ...

The cncjs user interface runs inside a browser program.  Modern browsers are enormous programs that use lots of memory and continually perform complex graphics display operations.  The cncjs user interface code uses recent Javascript features that only work on the rather new browsers.  The full cncjs UI depends on techniques that require lots of computation and memory, for example dynamic widget layouts that automatically scale for different size screens.  Its 3D viewer, which displays GCode toolpaths, needs a capable graphics processor that is well-supported by the operating system and browser.

A Raspberry Pi is just barely powerful enough for the full cncjs UI - but not powerful enough for a satisfying user experience.  The Pi3's graphics processor (GPU) is slow (400 MHz) and the other Pis are even slower (250 MHz).  The GPU shares memory with the CPU, and there isn't a lot of memory to start with(1 GB total on Pi3 and Pi2B, 512 MB total on PiB and Pi0).  The GPU is optimized for video, not 3D graphics.  The software support for GPU 3D graphics is so new that it listed as "experimental" in the most recent OS release (Raspbian Stretch) and must be enabled manually - it is not turned on by default, and it conflicts with some other software.  It works with version 60 of the Chromium browser, but it is common for display operations to take a long time, with the CPU usage pegged at 100%.  Firefox also seems to work, but is similarly sluggish.

The Epiphany browser does not support 3D viewing, but seems to work okay with 3D turned off.  It too is sluggish.  For example, after running a short GCode program, it takes 17 seconds for the "play pause stop close" icon bar to update.

The system tends to crash if you try to do too much - such as running Chromium and Firefox simultaneously.  That is probably caused by using up all the system memory.

The bottom line: As of this writing (Nov 2017), a Raspberry Pi is not well-suited for the full cncjs user interface with its 3D viewer enabled.  If you turn off the 3D viewer and just view the GCode as text, it seems to be okay, but some operations like starting up and establishing the serial connection can take awhile.

### Alternative CNCjs User Interfaces

There are some alternative CNCjs user interfaces that do not push the Raspberry Pi so hard - for example cncjs-pendant-tinyweb and cncjs-shopfloor-tablet.  They have far fewer features than the full CNCjs UI, but they may be enough for some use cases.

### Older Raspbian Releases

The current Rasbian release is called "stretch".  The one before that was "jessie", and before that "wheezy".  Trying to run the CNCjs UI on wheezy was an exercise in frustration - various components were so old that many things about CNCjs would not work at all.  Jessie was somewhat better, but it was difficult to get an up to date browser working with jessie.
The process of upgrading from wheezy to jessie, and then to stretch, took several days.