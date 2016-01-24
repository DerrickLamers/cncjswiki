## Getting Started

## Keyboard Shortcuts
These are the current keys used in the cnc.js (from v0.15.2).
* `!` - Feed Hold
* `~` - Resume
* `ctrl + alt + command + h` - Homing
* `ctrl + alt + command + u` - Unlock
* `ctrl + alt + command + r` - Reset
* `ctrl + alt + command + =` - Switch Jog Distance
* `ctrl + alt + command + [` - Jog Backward
* `ctrl + alt + command + ]` - Jog Forward
* `ctrl + alt + command + x` - Select/Deselect X Axis
* `ctrl + alt + command + y` - Select/Deselect Y Axis
* `ctrl + alt + command + z` - Select/Deselect Z Axis

## ShuttleXpress Jog Dial
You can use the ShuttleXpress jog dial to work with a CNC controller. The ShuttleXpress has five programmable buttons, a 10 counts jog dial (the inner wheel), and a 15-position shuttle wheel (the outer wheel) that returns to center when released.

![](https://raw.githubusercontent.com/cheton/cnc.js/dev/media/ShuttleXpress.jpg)

To work with cnc.js, configure three buttons to select/deselect X/Y/Z axis, and another one button to switch the distance value. Set turn jog dial left (CCW) to move backward/down, and set turn jog right (CW) to move forward/up.

#### An example of ShuttleXpress Settings:

Buttons
* Button 2 - Select/Deselect X Axis<br>
  Type key: `ctrl + alt + command + x`
* Button 3 - Select/Deselect Y Axis<br>
  Type key: `ctrl + alt + command + y`
* Button 4 - Select/Deselect Z Axis<br>
  Type key: `ctrl + alt + command + z`
* Button 5 - Switch Distance (1, 0.1, 0.01, 0.001, or custom value) <br>
  Type key: `ctrl + alt + command + =`

Jog
* Turn Jog Right - Move Forward/Up<br>
  Type key: `ctrl + alt + command + ]`
* Turn Jog Left - Move Backward/Down<br>
  Type key: `ctrl + alt + command + [`