This page contains issues people have had trying to resolve. Many problems have been solved before. Please take a look at the following resources for troubleshooting.

You may also want to visit the [FAQ](https://cnc.js.org/docs/faq/) if your questions are about common installation problems.

Please bear the following precautions in mind when troubleshooting:
* **NEVER PLUG OR UNPLUG COMPONENTS WHEN THEY ARE POWERED**
* **TURN OFF SPINDLE AND MOTOR POWER WHEN MAKING A DRY-RUN FOR TROUBLESHOOTING SERIAL TRANSMISSION**

## Table of Contents

* [Experienced unexpected errors during data transmission](#experienced-unexpected-errors-during-data-transmission)
* [Troubleshoot Electron app on Windows](#troubleshoot-electron-app-on-windows)
* [Compilation Problems](#compilation-problems)

## Experienced unexpected errors during data transmission

Some symptoms of data transmission issues may be:
  - Web interface stops updating Axis positions although the machine is still moving
  - A job finishes, but the web interface shows the job still in progress (the pause button is still lit and the start button is disabled)
  - The toolhead is in a different location than the GCode indicated
  - Console reports errors, but the sent line is valid GCode, as in this screenshot:

![Console Errors](https://cloud.githubusercontent.com/assets/25835568/23258430/387af688-f996-11e6-8de4-f76fdfd8fc28.png)

Possible resolutions for data transmission issues may be:
  - Update your Arduino firmware to address a known serial communication bug: https://github.com/grbl/grbl/wiki/Known-Bugs
  - Bypass any intermediate stops between the computer running cncjs and your Arduino (USB Hubs, Extension cables, etc.) and plug the Arduino directly into the computer running cncjs.
  - Use a shielded USB cable, at the shortest length possible to reduce electrical interference.
  - Install ferrite cores on each end of the USB cable, or use a shielded USB cable that has built-in ferrite cores.
  - Turn off any nearby electrical components, unplug them, and move them away from the area.  Any piece of metal nearby can act as an antenna for electrical interference.
  - Separate your motor driver cables and spindle cables from your signal cables (USB and limit switch wires).  The current flowing through the motor and spindle cables can induce electrical signals into nearby cables, causing interference.  This may be more difficult than it seems.  Basically, just make sure they don't run parallel while touching.  If they do touch (like at a mounting point), make sure they cross and go on different paths.
  
## Troubleshoot Electron app on Windows

1. Go to https://github.com/electron/electron/releases, and download a zip file that matches your target platform. For example, you can download `electron-{version}-win32-x64.zip` when running the x64 version of Windows.
2. Extract the ZIP file to a folder. For example: <b>C:\\Temp\\electron-{version}-win32-x64\\</b>.
3. Change current working directory to <b>C:\\Users\{Username}\\AppData\\Local\\Programs\\cncjs-app\\resources\\app\\</b>.

    ![image](https://user-images.githubusercontent.com/447801/58223054-c0f75f80-7d4a-11e9-8328-404571d5a25c.png)

4. Run <b>C:\\Temp\\electron-{version}-win32-x64\\electron.exe main.js -vvv</b>, then you will be able to see verbose output with `-vvv`.

    ![image](https://cloud.githubusercontent.com/assets/447801/24361796/7c4bc25a-133d-11e7-80f7-07392c175899.png)

## Compilation Problems

CNCjs can be tricky to compile.  It is a large body of Javascript code that has many dependencies on external libraries and frameworks.  The compilation tools and the project are configured to compile well on powerful computers with good network connectivity.  Compiling on systems with limited resources, such as a Raspberry Pi, or an entry-level cloud container, or a computer with a slow network connection, can be challenging. Sometimes the compilation (with "npm install") will run for a long time, only to fail late in the process.  Here are some situations that have arisen:

### ETIMEDOUT

Sometimes the compilation fails and, near the end, these lines appear:

```
npm ERR! network connect ETIMEDOUT
npm ERR! network This is most likely not a problem with npm itself
npm ERR! network and is related to network connectivity.
```

When I had that problem, it turned out that the compiler tool ("npm") was trying to use a lot of network connections at once, in hopes of speeding up the process of fetching dependencies by doing a lot of it in parallel.  Unfortunately, that overloaded some server or router, which just refused to respond anymore.  The solution is to tell npm to be less aggressive:

```
$ npm set maxsockets 3
```
The default value for maxsockets is 50.  Dialing it down to 3 solved all of my ETIMEDOUT problems, without slowing things down much.  You might try some intermediate values if 3 seems too conservative.

### ELIFECYCLE

Sometimes a compile runs for a long time then fails with messages that include

```
error code ELIFECYCLE
```

I have diagnosed at least one of these failures to an "out of memory" condition on the computer.  The compiler (in this case, typically the "webpack" component) wants to use more memory than the system has.  This is especially a problem with free-plan cloud containers that have only 256MB of RAM.

Sometimes you can work around this by splitting up the compilation into substeps.  The cncjs compilation configuration tries to run some steps in parallel, which saves time on sufficiently powerful computers, but needs more memory.  Instead doing everything automatically with "npm install", you can run the last phases separately, with

```
$ npm run build-prod-app
$ npm run build-prod-web
```
Sometimes that works.


