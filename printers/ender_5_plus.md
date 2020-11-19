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
1. Heat up nozzle to 200*C
2. Mark 10cm from feeder, adding 1cm for another mark
```
->>-----(110mm)-----(100mm)-------------------(0mm measure from here)||(feeder)||--------------
```
2. Menu > Move extruder > 100mm
For example, it did not consume 100mm (as marked on the filament):
- Check Menu > Motion > Estep/mm: for example 93mm/sec
- Theory input length (100) x 93(e-step number) = 9300
- Actual input length: 89mm => 9300/89 = 104.4 mm/sec

3. Change the estep to new value, run test  again.

4. Print test cube, 0.4 nozzle, 2line thick ==> wall should be 0.8mm.
If it's bigger -> over extrusion
If it's smaller -> under extrusion

For example: got 0.92 / 0.8 = 88.8% 
Change *FLOW setitng* in SLICER to 88% to get the perfect size.

