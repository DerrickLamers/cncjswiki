## Getting Started

## Keyboard Shortcuts
These are the current keys used in the cnc.js (from v0.15.3).<br>
<kbd>!</kbd> - Feed Hold<br>
<kbd>~</kbd> - Resume<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>h</kbd> - Homing<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>u</kbd> - Unlock<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>r</kbd> - Reset<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>x</kbd> - Select/Deselect X Axis<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>y</kbd> - Select/Deselect Y Axis<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>z</kbd> - Select/Deselect Z Axis<br>
<kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>=</kbd> - Switch Jog Distance<br>
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

## ShuttleXpress Jog Dial
You can use the ShuttleXpress jog dial to work with a CNC controller. The ShuttleXpress has five programmable buttons, a 10 counts jog dial (the inner wheel), and a 15-position shuttle wheel (the outer wheel) that returns to center when released.

![](https://raw.githubusercontent.com/cheton/cnc.js/dev/media/ShuttleXpress.jpg)

To work with cnc.js, configure three buttons to select/deselect X/Y/Z axis, and another one button to switch the distance value. Set turn jog dial left (CCW) to jog backward/down, and set turn jog right (CW) to jog forward/up.

### ShuttleXpress Settings

#### Buttons
* Button 2 - Select/Deselect X Axis<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>x</kbd>
* Button 3 - Select/Deselect Y Axis<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>y</kbd> 
* Button 4 - Select/Deselect Z Axis<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>z</kbd> 
* Button 5 - Switch Jog Distance (1, 0.1, 0.01, 0.001, or a custom value)<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>=</kbd> 

#### Jog Wheel
* Turn Jog Left<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>[</kbd> 
* Turn Jog Right<br>
  <kbd>ctrl</kbd> + <kbd>alt</kbd> + <kbd>command</kbd> + <kbd>]</kbd> 

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