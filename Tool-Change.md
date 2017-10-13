### **_The Tool Change function is only available for CNCjs 1.9.11 or later versions_**

## Getting Started

The manual tool change operation is initiated by a M6 command, it will pause program execution and prompt to change the tool. Read the following paragraphs to learn how to create a tool change macro to facilitate the process.

```
M6 ; Tool Change
```

![image](https://user-images.githubusercontent.com/447801/31429228-180a3740-ae33-11e7-89db-d1bb83cf7670.png)

## Initial Setup

### 1. Determine the tool change and the tool probe location

The tool change macro requires you to specify the tool change and the tool probe location in absolute coordinates. You can jog to the desire location and specify the position in your tool change macro with macro variables. It's recommended that you use upper case letters in variable names or prefix variable names with "_" or "$" to avoid conflict with reserved variables. The expression must be prefixed with "%" at the beginning of each line. For example:

```
%CLEARANCE_HEIGHT = 100
%TOOL_CHANGE_X = -300
%TOOL_CHANGE_Y = -300
%TOOL_CHANGE_Z = CLEARANCE_HEIGHT
%TOOL_PROBE_X = 0
%TOOL_PROBE_Y = 0
%TOOL_PROBE_Z = 20
```

### 2. Create a tool change macro

The tool change macro will perform the following operations:

* Set user-defined variables
  ```
  %CLEARANCE_HEIGHT = 100
  %TOOL_CHANGE_X = -300
  %TOOL_CHANGE_Y = -300
  %TOOL_CHANGE_Z = CLEARANCE_HEIGHT
  %TOOL_PROBE_X = 0
  %TOOL_PROBE_Y = 0
  %TOOL_PROBE_Z = 20
  %PROBE_DISTANCE = 15
  %PROBE_FEEDRATE = 20
  %TOUCH_PLATE_HEIGHT = 10
  %RETRACTION_DISTANCE = 10
  ```

* Keep a backup of current work position
  ```
  %X0 = posx, Y0 = posy, Z0 = posz
  ```

* Save modal state
  ```
  ; Work Coordinate System: G54, G55, G56, G57, G58, G59
  %WCS = modal.wcs

  ; Plane: G17, G18, G19
  %PLANE = modal.plane

  ; Units: G20, G21
  %UNITS = modal.units

  ; Distance Mode: G90, G91
  %DISTANCE = modal.distance

  ; Feed Rate Mode: G93, G94
  %FEEDRATE = modal.feedrate

  ; Spindle State: M3, M4, M5
  %SPINDLE = modal.spindle

  ; Coolant State: M7, M8, M9
  %COOLANT = modal.coolant
  ```

* Stop spindle

* Raise to tool change Z (clearance height)

* Go to tool change X,Y

* Pause the program with `M0` (pause w/ hold) for a manual tool change

* Click <kbd>Continue</kbd> to resume

  ![image](https://user-images.githubusercontent.com/447801/31432194-89c6fc26-ae3b-11e7-8e5f-eea4cfffc99a.png)

* Go to tool probe X,Y

* Lower to tool probe Z

* Pause the program with `M1` (pause w/o hold) before probing

* Click <kbd>Continue</kbd> to resume
  
  ![image](https://user-images.githubusercontent.com/447801/31432229-a1c2a0e6-ae3b-11e7-9ce2-1bbfecc17648.png)

* Perform a probe toward workpiece with a maximum probe distance

* Set Z0 for the active work coordinate system

* Raise to tool change Z

* Go to tool change X,Y

* Pause the program with `M0` for cleanup (e.g. remove touch plate, wires, etc)

* Click <kbd>Continue</kbd> to resume

  ![image](https://user-images.githubusercontent.com/447801/31432194-89c6fc26-ae3b-11e7-8e5f-eea4cfffc99a.png)

* Go to previous work position
  ```
  G0 X[X0] Y[Y0]
  G0 Z[Z0]
  ```

* Restore modal state
  ```
  [WCS] [PLANE] [UNITS] [DISTANCE] [FEEDRATE] [SPINDLE] [COOLANT]
  ```

* Continue program execution

#### The Tool Change Macro

Below is an example of the tool change macro, you can customize the macro for your needs:

```
; Wait until the planner queue is empty
%wait

; Set user-defined variables
%CLEARANCE_HEIGHT = 100
%TOOL_CHANGE_X = -300
%TOOL_CHANGE_Y = -300
%TOOL_CHANGE_Z = CLEARANCE_HEIGHT
%TOOL_PROBE_X = 0
%TOOL_PROBE_Y = 0
%TOOL_PROBE_Z = 20
%PROBE_DISTANCE = 15
%PROBE_FEEDRATE = 20
%TOUCH_PLATE_HEIGHT = 10
%RETRACTION_DISTANCE = 10

; Keep a backup of current work position
%X0=posx, Y0=posy, Z0=posz

; Save modal state
; * Work Coordinate System: G54, G55, G56, G57, G58, G59
; * Plane: G17, G18, G19
; * Units: G20, G21
; * Distance Mode: G90, G91
; * Feed Rate Mode: G93, G94
; * Spindle State: M3, M4, M5
; * Coolant State: M7, M8, M9
%WCS = modal.wcs
%PLANE = modal.plane
%UNITS = modal.units
%DISTANCE = modal.distance
%FEEDRATE = modal.feedrate
%SPINDLE = modal.spindle
%COOLANT = modal.coolant

; Stop spindle
M5
; Absolute positioning
G90

; Raise to tool change Z
G0 Z[TOOL_CHANGE_Z]
; Go to tool change X,Y
G0 X[TOOL_CHANGE_X] Y[TOOL_CHANGE_Y]
; Wait until the planner queue is empty
%wait

; Pause the program for a manual tool change
M0

; Go to tool probe X,Y
G0 X[TOOL_PROBE_X] Y[TOOL_PROBE_Y]
; Lower to tool probe Z
G0 Z[TOOL_PROBE_Z]
; Wait until the planner queue is empty
%wait

; Pause the program before probing
M1

; Probe toward workpiece with a maximum probe distance
G91 ; Relative positioning
G38.2 Z-[PROBE_DISTANCE] F[PROBE_FEEDRATE]
G90 ; Absolute positioning

; Set Z0 for the active work coordinate system
G10 L20 P0 Z[TOUCH_PLATE_HEIGHT]

; Retract from the touch plate
G91 ; Relative positioning
G0 Z[RETRACTION_DISTANCE]
G90 ; Absolute positioning

; Raise to tool change Z
G0 Z[TOOL_CHANGE_Z]
; Go to tool change X,Y
G0 X[TOOL_CHANGE_X] Y[TOOL_CHANGE_Y]
; Wait until the planner queue is empty
%wait

; Pause the program for cleanup (e.g. remove touch plate, wires, etc)
M0

; Go to previous work position
G0 X[X0] Y[Y0]
G0 Z[Z0]

; Restore modal state
[WCS] [PLANE] [UNITS] [DISTANCE] [FEEDRATE] [SPINDLE] [COOLANT]
```

Use the following codes if you want to update the tool length offset (TLO) instead of adjusting the current WCS: 

```
; Cancel tool length offset
G49

; Probe toward workpiece with a maximum probe distance
G91 ; Relative positioning
G38.2 Z-[PROBE_DISTANCE] F[PROBE_FEEDRATE]
G90 ; Absolute positioning

; A dwell time of one second to make sure the planner queue is empty
G4 P1

; Update the tool length offset
G43.1 [posz - TOUCH_PLATE_HEIGHT]

; Retract from the touch plate
G91 ; Relative positioning
G0 Z[RETRACTION_DISTANCE]
G90 ; Absolute positioning
```

## M6 Tool Change

When a M6 command is issued, CNCjs will pause program execution and prompt user the change the tool. You can run the above tool change macro to perform a manual tool change, and click ▶ (Resume) to continue.

![image](https://user-images.githubusercontent.com/447801/31432483-5b8e0470-ae3c-11e7-8ff3-035dce7ceec4.png)