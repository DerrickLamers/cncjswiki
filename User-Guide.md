## index

* [Workspace](#workspace)
  - [Widgets](#widgets)
* [Settings](#settings)
  - [General](#general)
  - [Workspace](#workspace)
  - [My Account](#my-account)
  - [Commands](#commands)
  - [Events](#events)
* [Keyboard Shortcuts](#keyboard-shortcuts)
* [Contour ShuttleXpress](#contour-shuttlexpress)

---

## Workspace

![](https://raw.githubusercontent.com/cncjs/cncjs/master/media/widgets.png)

### Visualizer Widget

This widget visualizes a G-code file and simulates the tool path.

### Connection Widget

This widget lets you establish a connection to a serial port.

### Axes Widget

This widget shows the XYZ position. It includes jog controls, homing, and axis zeroing.

### Console Widget

This widget lets you read and write data to the CNC controller connected to a serial port.

### G-code Widget

This widgets shows the current status of G-code commands.

### Grbl Widget

This widet shows the Grbl state and provides Grbl specific features.

Set `$10=2` for Grbl v1.1d (or `$10=15` for Grbl v0.9) to see planner buffer and receive buffer in queue reports.

![](https://cloud.githubusercontent.com/assets/447801/20649442/5912660e-b4fb-11e6-8ff8-60b2602f5d79.png)

### Smoothie Widget

This widget shows the Smoothie state and provides Smoothie specific features.

### TinyG widget

This widget shows the TinyG state and provides TinyG specific features.

### Laser Widget

This widget allows you control laser intensity and turn the laser on/off.

### Macro Widget

This widget can use macros to automate routine tasks.  The body of a macro is a GCode program, but you can replace parts of the GCode with calculated values.  The calculations can be performed on literal numbers or on values representing current machine positions. You can create variables - names that represent values - and use them in calculations or replacements.

The characters that distinguish regular GCode from the extra CNCjs macro syntax are % , [ , and ] .  Lines beginning with % are used to create variables for later use.  Inside a GCode line, [ _expression_ ]  is replaced with the value of _expression_

The GCode program must be suitable for the controller (Grbl, TinyG, Marlin, etc) that you are using.  There are subtle difference in the dialect of GCode that the different controllers support.  For example, Marlin only accepts capital letters like `G0`, while Grbl will accept either `G0` or `g0`.

The macro language can be used in GCode programs loaded from files too, not only from the Macro Widget.

#### Creating Variables

Lines that start with `%` (except for the special case `%wait`) create variables that can be used later. You can set a variable to the value of an arithmetic expression.  Inside an expression, you can use variables that you created earlier, and predefined system variables that report the state of the machine.  The syntax of expressions is a subset of the JavaScript programming language expression syntax.

```
%p1x = posx
%p1y = posy
%p2x = posx
%p2y = posy
%p3x = posx
%p3y = posy
%ma = (p2y - p1y) / (p2x - p1x)
%mb = (p3y - p2y) / (p3x - p2x)
%cx = (ma * mb * (p1y - p3y) + mb * (p1x + p2x) - ma * (p2x + p3x)) / (2 * (mb - ma))
%cy = (-1 / ma) * (cx - (p1x + p2x) * 0.5) + (p1y + p2y) * 0.5
```
You can declare more than one variable on the same line by using comma to separate the variables, as with:
```
%p2x=posx, %p2y=posy
```

`%wait` is a special word.  CNCjs replaces it with code to make the controller wait until it has finished all previous operations.

#### Replacements

When CNCjs encounters a pair of square brackets [ _expression_ ] in GCode, it replaces it with the value of 
_expression_.  The following sequence:
```
%cx = 10
%cy = 10
G0 X[cx] Y[cy]
```
results in this being sent to the controller:
```
G0 X10 Y10
```

The text inside the brackets can be an expression, not just a simple variable.  You could say, for example:
```
%scale = 0.4
%cx = 10
%cy = 10
G0 X[cx*scale] Y[cy*scale]
```
which would send this to the controller:
```
G0 X4 Y4
```

#### System Variables
There are some predefined variables that report the current state of the machine and the GCode program.

##### Bounding Box
`xmin`, `xmax`, `ymin`, `ymax`, `zmin`, `zmax`

##### Machine Position
`mposx`, `mposy`, `mposz`, `mposa`, `mposb`, `mposc`

##### Work Position
`posx`, `posy`, `posz`, `posa`, `posb`, `posc`

##### Modal Group
The values of the following system variables are not numbers, but rather GCode words like `G90`.  They can be used to save the current modal state of the controller and then later restore that state.  For example, `modal.units` is either `G20` for inches or `G21` for millimeters.  You could write a macro that works in inches, regardless of the current controller setting, then, at the end, restore the controller setting to whatever it was before.

`modal.motion`, `modal.wcs`, `modal.plane`, `modal.units`, `modal.distance`, `modal.feedrate`, `modal.program`, `modal.spindle`, `modal.coolant`

#### Examples

* Wait until the planner queue is empty
  ```
  %wait
  ```

* Print the value of a variable in the console
  ```
  %X0=posx
  (X0=[X0]) ; Print the value in the inline comment
  G4 P0 (X0=[X0]) ; Print the value in the inline comment right after a G4 dwell command
  ```

* Save the current work position for later
  ```
  %X0=posx, Y0=posy, Z0=posz, A0=posa, B0=posb, C0=posc
  ```

* Go to previous work position
  ```
  G0 X[X0] Y[Y0]
  G0 Z[Z0]
  ```

* Saving and restoring modal state
This would go at the beginning of the macro, to record the current modal state in variables:
  ```
  %WCS=modal.wcs
  %PLANE=modal.plane
  %UNITS=modal.units
  %DISTANCE=modal.distance
  %FEEDRATE=modal.feedrate
  %SPINDLE=modal.spindle
  %COOLANT=modal.coolant
  ```
During the execution of the macro, the modal state (and the values of the modal special variables) might change as a result of GCode words inside the macro.  At the end of the macro, you could write this to restore the modes to their saved values:

  ```
  [WCS] [PLANE] [UNITS] [DISTANCE] [FEEDRATE] [SPINDLE] [COOLANT]
  ```
Note that Marlin does not permit multiple GCode words on the same line, so if you are using Marlin, you would have to put each of the above bracketed expressions on a separate line.

* Set bounding box
  ```
  %xmin=0,xmax=100,ymin=0,ymax=100,zmin=0,zmax=50
  ```

* Traverse around the boundary
  ```
  G90
  G0 Z10 ; go to z-safe
  G0 X[xmin] Y[ymin]
  G0 X[xmax]
  G0 Y[ymax]
  G0 X[xmin]
  G0 Y[ymin]
  ```

  Once a G-code file is loaded, run the macro for perimeter tracing with respect to current G-code boundary.

  ![image](https://cloud.githubusercontent.com/assets/447801/24189183/f924b7aa-0f1e-11e7-8b6f-64a14c23d441.png)

### Probe Widget

This widget helps you use a touch plate to set your Z zero offset.

### Spindle Widget

This widget provides the spindle control.

### Webcam Widget

This widget lets you monitor a webcam.

Checkout [FAQ](https://cnc.js.org/docs/faq/) to learn how to setup and configure webcam streaming with Raspberry Pi.

---

## Settings

### General

### Workspace

### My Account

![image](https://cloud.githubusercontent.com/assets/447801/20375707/d036adbe-acbb-11e6-8b3f-514c06e7c9e4.png)

### Commands

### Events

---

## Keyboard Shortcuts
These are the current keys used in the cnc (from v0.15.3).<br>
<kbd>!</kbd> - Feed Hold<br>
<kbd>~</kbd> - Resume<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>h</kbd> - Homing<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>u</kbd> - Unlock<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>r</kbd> - Reset<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>x</kbd> - Select/Deselect X Axis<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>y</kbd> - Select/Deselect Y Axis<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>z</kbd> - Select/Deselect Z Axis<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>a</kbd> - Select/Deselect A Axis<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>b</kbd> - Select/Deselect B Axis (_Supported in v1.9.15_)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>c</kbd> - Select/Deselect C Axis (_Supported in v1.9.15_)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>=</kbd> - Toggle Jog Distance<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>+</kbd> - Increase Jog Distance (_Supported in v1.9.15_)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>-</kbd> - Decrease Jog Distance (_Supported in v1.9.15_)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>[</kbd> - Jog Backward<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>]</kbd> - Jog Forward<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>shift</kbd> + <kbd>7</kbd> - Shuttle Backward (Fastest)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>shift</kbd> + <kbd>6</kbd> - Shuttle Backward (Faster)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>shift</kbd> + <kbd>5</kbd> - Shuttle Backward (Fast)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>shift</kbd> + <kbd>4</kbd> - Shuttle Backward (Normal)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>shift</kbd> + <kbd>3</kbd> - Shuttle Backward (Slow)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>shift</kbd> + <kbd>2</kbd> - Shuttle Backward (Slower)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>shift</kbd> + <kbd>1</kbd> - Shuttle Backward (Slowest)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>0</kbd> - Shuttle Stop<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>1</kbd> - Shuttle Forward (Slowest)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>2</kbd> - Shuttle Forward (Slower)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>3</kbd> - Shuttle Forward (Slow)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>4</kbd> - Shuttle Forward (Normal)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>5</kbd> - Shuttle Forward (Fast)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>6</kbd> - Shuttle Forward (Faster)<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>7</kbd> - Shuttle Forward (Fastest)<br>

---

## Contour ShuttleXpress
You can use the ShuttleXpress jog dial to work with a CNC controller. The ShuttleXpress has five programmable buttons, a 10 counts jog dial (the inner wheel), and a 15-position shuttle wheel (the outer wheel) that returns to center when released.

![](https://raw.githubusercontent.com/cncjs/cncjs/dev/media/ShuttleXpress.jpg)

To work with cnc, configure three buttons to select/deselect X/Y/Z axis, and another one button to switch the distance value. Set turn jog dial left (CCW) to jog backward/down, and set turn jog right (CW) to jog forward/up.

### ShuttleXpress Settings

#### Buttons
* Button 1 - Select/Deselect A Axis<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>a</kbd>
* Button 2 - Select/Deselect X Axis<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>x</kbd>
* Button 3 - Select/Deselect Y Axis<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>y</kbd> 
* Button 4 - Select/Deselect Z Axis<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>z</kbd> 
* Button 5 - Select Jog Distance (1, 0.1, 0.01, 0.001, or a custom value)<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>=</kbd> or <kbd>d</kbd>

#### Jog Wheel
* Jog Backward<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>[</kbd> or <kbd>b</kbd>
* Jog Forward<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>]</kbd> or <kbd>f</kbd>

#### Shuttle Wheel

Adjust the keystroke repeat rate to **10 times per second** for all Shuttle Zones except the Shuttle Zone 0.

* Shuttle Zone -7<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>shift</kbd> + <kbd>7</kbd>
* Shuttle Zone -6<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>shift</kbd> + <kbd>6</kbd>
* Shuttle Zone -5<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>shift</kbd> + <kbd>5</kbd>
* Shuttle Zone -4<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>shift</kbd> + <kbd>4</kbd>
* Shuttle Zone -3<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>shift</kbd> + <kbd>3</kbd>
* Shuttle Zone -2<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>shift</kbd> + <kbd>2</kbd>
* Shuttle Zone -1<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>shift</kbd> + <kbd>1</kbd>
* Shuttle Zone 0<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>0</kbd>
* Shuttle Zone 1<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>1</kbd>
* Shuttle Zone 2<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>2</kbd>
* Shuttle Zone 3<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>3</kbd>
* Shuttle Zone 4<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>4</kbd>
* Shuttle Zone 5<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>5</kbd>
* Shuttle Zone 6<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>6</kbd>
* Shuttle Zone 7<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>7</kbd>

#### Reduce humming sound when accelerating and decelerating
If you'd hear a strange humming noise during acceleration and deceleration, try to increase the max feed rate and the acceleration speed, and make sure it will not miss steps. Use '$'-commands to tweak Grbl system settings, like below:
```
> $$
   :    :
$110=2500.000 (x max rate, mm/min)
$111=2500.000 (y max rate, mm/min)
$112=500.000 (z max rate, mm/min)
$120=250.000 (x accel, mm/sec^2)
$121=250.000 (y accel, mm/sec^2)
$122=50.000 (z accel, mm/sec^2)
   :    :
ok
> $110=2500.000
ok
> $120=250.00
ok
```
In the general case, higher acceleration (mm/sec^2) can significantly reduce humming sound when accelerating and decelerating, but you may need to adjust various settings with your CNC machine. To adjust ShuttleXpress specific settings, click on the <kbd><img src="https://cdn.rawgit.com/cncjs/cncjs/master/media/font-awesome/black/svg/cog.svg" width="16" title="Edit" /></kbd> button at the top of the Axes widget.

![](https://raw.githubusercontent.com/cncjs/cncjs/master/media/widgets/axes-settings.png)
* Feed Rate Range: 100-2500 mm/min (default: 500-2000 mm/min)
  - Defines the minimum feed rate for Shuttle Zone +1 and -1
  - Defines the maximum feed rate for Shuttle Zone +7 and -7
* Repeat Rate: 60Hz - 1Hz (default: 10Hz)
  - The repeat rate should be equal to your keystroke repeat rate for each Shuttle Zones. 
* Distance Overshoot: 1x - 1.5x (default: 1x)
  - Defines the overshoot of the travel distance