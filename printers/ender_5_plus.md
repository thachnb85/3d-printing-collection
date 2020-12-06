# 1. Upgrade for Ender 5 Plus

# 2. Setup and troubleshooting
Performing below steps for the new printer
## 2.1. Manual leveling
- Adjust 2 z-axis using caliper to reach the same height on the left/right z-axis
- After that, using the 4 knobs under printing bed to level with manual movement of the hotend (gently).
## 2.2. Auto leveling
- After done 2.1., go to:
Menu > Print > select any model on the SD Card > change z-offset to 0.
- Go to Menu > Setup > Leveling
- Now adjusting the z-offset using up/down button.
- Save z-home by press z-home.

# 3. Optimizing printing surface
## 3.1. Slicing software optimization
- Printer profile should be correct in Cura
- Profile > MeshFixes: choose 0.5 instead of 0.005 for smoother surface.

# 4. Extruder calibration
### 2. Mark 12cm from feeder, adding 1cm for another mark
```
->>-----(120mm)------(0mm measure from here)||(feeder)||--------------
```

### 3. Measurement using command 
Set temp of nozzle
```
M104 S220
```

Enable Relative mode
Using Serial debug or Octoprint terminal.
```
M83
```

Check current e-value:
```
M503
```
Check value of line echo: M92 E___

Extrude 10cm
```
G1 E100 F100
```

If the measurement is 20 mm, your extruder is properly calibrated, and you do not need to continue following these instructions.

If your measurement is more than 20 mm, your printer is likely suffering from under-extrusion, and your steps/mm setting needs to be increased.

If your measurement is less than 20 mm, your printer is over-extruding. This means that you need to turn down the steps/mm setting.

Actual length extruded = 120 -  [length from extruder to mark]
[steps/mm value] x 100 = [steps taken]
=> [steps taken] / [actual length extruded] = [accurate steps/mm value]

Example over extruding case: 
```
measured 21mm
120 - 15 = 105 mm
[steps taken] = 140 (current) * 100 = 14000
[accurate steps/mm value] = 14000/105 = 133
```

Example under-extruding case: 
```
measure 21mm, 120 - 21 = 99 mm
[steps taken] = 140 (current) * 100 = 14000
[accurate steps/mm value] = 14000/99 = 141
```

Set new e-step:
```
M92 133
```

Save to memory
```
M500
```

### 4. Print test cube
0.4 nozzle, 2line thick ==> wall should be 0.8mm.
If it's bigger -> over extrusion
If it's smaller -> under extrusion

=> If we don't want to calibrate, using this value in slicer flow rate:
For example: got 0.92 / 0.8 = 88.8% 
Change *FLOW setitng* in SLICER to 88% to get the perfect size.

# 5. Large nozzle
 CURA settings for large nozzle:
 - Infill: density 5
 - Connect infill line CHECKED
 - Gradual infill steps 0
