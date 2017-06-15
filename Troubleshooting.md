This page contains issues people have had trying to resolve. Many problems have been solved before. Please take a look at the following resources for troubleshooting.

You may also want to visit the [FAQ](https://cnc.js.org/docs/faq/) if your questions are about common installation problems.

Please bear the following precautions in mind when troubleshooting:
* **NEVER PLUG OR UNPLUG COMPONENTS WHEN THEY ARE POWERED**
* **TURN OFF SPINDLE AND MOTOR POWER WHEN MAKING A DRY-RUN FOR TROUBLESHOOTING SERIAL TRANSMISSION**

## Table of Contents

* [Experienced unexpected errors during data transmission](#experienced-unexpected-errors-during-data-transmission)
* [Troubleshoot Electron app on Windows](#troubleshoot-electron-app-on-windows)

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
3. Go to the installation directory of cncjs: `cd C:\Users\{Username}\AppData\Local\Programs\cncjs\resources\app\`
4. Run <b>C:\\Temp\\electron-{version}-win32-x64\\electron.exe main.js -vvv</b>, then you will be able to see verbose output with `-vvv`.

    ![image](https://cloud.githubusercontent.com/assets/447801/24361796/7c4bc25a-133d-11e7-80f7-07392c175899.png)