# Ampler Strout Gen 2 Head Unit

I have a nearly working head unit, except that the display no longer works because the cable connecting the display to the main unit has failed. Bluetooth seems to be working, but the previous owner had issues with the app and lost their login credentials, making it impossible to transfer the bike to my account.

I’m currently looking for ways to replace or repair the unit, and I thought I’d share some of my findings along the way in case they might be useful to others facing a similar issue.

## Removing the Head-Unit

The unit is connected to the bike with three cables, as shown [here](https://www.youtube.com/watch?v=6S0qXE2csM8).
The main connector appears to be a **HIGO-B9-F** (male) and **HIGO-S9-F** (female).

## The Display

In my case, only the display backlight was working.
After removing the single screw and carefully peeling away some of the silicone, I was able to separate the display from the rest of the head unit.
Unfortunately, during the process I seem to have damaged the display even further. When I connect it now, it shows some strange/funky pixels. This isnt surprising, as I had already noticed some damage beforehand, and quite a bit of force was required to remove the silicone holding the display in place.
The display is connected via an FPC (Flexible Printed Circuit), which I disconnected from the head unit. Upon inspecting the cable, I noticed a [black hole in the middle of the pcb](media/BurnMarksOnCable.jpeg) and quickly concluded that the FPC was beyond repair.
After removing the display, I also removed the remaining transparent plastic that holds the different layers of the display assembly together. This revealed that the display is soldered directly to the FPC. Unfortunately, the display had already sustained significant damage during disassembly and would also need to be replaced even if it might still work.

After a quick Google search, I concluded that the cable is a custom design by Ampler. I didn't spend too much time searching, so it's possible that a replacement is available somewhere. 

Since the display is also in questionable condition and I'm locked out of the app anyway, replacing the entire head unit seems like the more sensible option.

## The Plan
For now, I want to build a simple replacement for the head unit with minimal functionality. My main goals are:
- Control over the lights
- A battery level indicator
- Basic control over the motor
- Optionally, support for multiple assist profiles
- Optionally, a speed indicator

The project can be broken down into three main steps:
1. Identify the internal communitcation and determin if the signals are digital or analog.
2. Find hardware that would fit in the space where the old head unit was.
3. Some programming to make it come together.

# Internal Communication 
After some probing with a multimeter i found the following voltages on the lines

Black -> gnd
Red -> battery voltage
White -> battery voltage*
Blue -> 6V*
Purple -> 6V*
Yellow -> 2.5V
Green -> 2.5V
Orange -> 2.4V*
Brown -> 2.4V*

The lines marked with a * idle at around 0V when the light/controller is off. 

Next, I connected my oscilloscope to inspect the signals, which were hovering around 2.5 V and 2.4 V. Based on these voltage levels and the differential signaling, I quickly identified the interfaces as CAN bus.

Interestingly, there seem to be two CAN buses. One is always on, while the other only comes alive when the controller is switched on. The green (CAN-H) and yellow (CAN-L) wires belong to the always-on bus, while brown (CAN-H) and orange (CAN-L) belong to the other. Both run at 250 kbit/s.


