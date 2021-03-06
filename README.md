# Online Video Looper Pedal
This is a DIY clone of the [Vidami Pedal](https://utility.design) by Utility Design giving you handsfree looping and playback control on **Youtube** and **Vimeo** videos. It features one more button and also a bank switch to switch between 2 sets of keymaps. I personally use bank 2 for mapping the foot switches to specific functions in my DAW.

## Reviews of the Vidami Pedal
- [Review by R.J. Ronquillo](https://www.youtube.com/watch?v=VKHbrlyeI6I)
- [Review by Steve Stine](https://www.youtube.com/watch?v=Oeq5DZQMakY)

## Parts to get
- 1x [Arduino Pro Micro](https://www.amazon.de/dp/B01KMFRCN0/)
- 1x [1590XX](https://www.taydaelectronics.com/catalogsearch/result/?q=1590xx) or [1590DD](https://www.taydaelectronics.com/catalogsearch/result/?q=1590dd) enclosure
- 6x [Soft momentary footswitches](https://www.taydaelectronics.com/spst-momentary-soft-touch-push-button-stomp-foots-pedal-switch.html)
- 1x [Toggle Switch On-On](https://www.taydaelectronics.com/mini-toggle-switch-1m-series-spdt-on-on-short-lever.html)
- 7x [10kOhm resistors](https://www.taydaelectronics.com/10k-ohm-1-2w-5-carbon-film-resistor.html)
- 1x [USB cable (USB-A to Micro-USB)](https://www.amazon.de/dp/B019Q6Y2MK/)
- some wire for soldering it all together

## Hardware Installation

### Arduino
 - check the pin layout of your Arduino (Pro) Micro
    - **NOTE**: the one linked above is different from the official one
    - [pinout for above linked Arduino Pro Micro](https://github.com/sparkfun/Pro_Micro/raw/master/Documentation/ProMicro16MHzv2.pdf)
    - [latest pinout for official Arduino Micro](https://content.arduino.cc/assets/Pinout-Micro_latest.png)
 - connect / solder wires to
    - 5V pin
    - GND Pin
    - 7 digital inputs
      - e.g. 5, 6, 7, 8, 9, 10, 16
      - if you choose custom ones you will have to modify the Arduino code
 - connect the other ends to your footswitches and resistors as shown in the following schematic:
 
 ![Schematic](https://github.com/andreasbrett/OVLP/raw/master/Schematic/schematic.png)

### Enclosure
 - drill 6 holes for the footswitches
 - drill 1 hole for the toggle switch
 - drill a hole for the USB cable
 - mount the Arduino Micro so it won't move (e.g. a [small breadboard](https://www.taydaelectronics.com/catalogsearch/result/?q=170+point+breadboard))
 - mount the footswitches
 - mount the toggle switch

## Software Installation
 - load and flash [OVLP.ino](Arduino/OVLP/OVLP.ino) onto the Arduino using [Arduino IDE 1.8.13 or newer](https://www.arduino.cc/en/Main/Software)
	- install library [Keyboard](https://www.arduino.cc/reference/en/language/functions/usb/keyboard/) 1.0.2 or newer
	- install library [MIDI Library](https://github.com/FortySevenEffects/arduino_midi_library) 5.0.2 or newer
	- install library [USB-MIDI](https://github.com/lathoub/Arduino-USBMIDI) 1.1.2 or newer
 - install [Tampermonkey extension](https://www.tampermonkey.net/) in Chrome/Firefox/Edge/Opera/Safari
 - import [OVLP.user.js](Tampermonkey/OVLP.user.js) into Tampermonkey

## Usage
Use the toggle switch to switch between bank 1 and 2 enabling different keystrokes / MIDI messages that the pedal will send via USB to your computer.

 - bank 1 is for Youtube / Vimeo (and can be used for other applications that support custom keyboard shortcuts)
   - footswitch **Play / Pause**
      - short press will toggle between play and pause
	  - long press will reset the playback rate and clear the loop
	  - sends Shift+Alt+S (Shift+Alt+Q for long press)
   - footswitch **Loop**
      - 1st time: sets start of the loop
      - 2nd time: sets end of the loop
      - 3rd time: clears loop
	  - sends Shift+Alt+W (Shift+Alt+T for long press)
   - footswitch **Back**
      - short press will jump back 5sec
	  - long press will rewind the video
	  - sends Shift+Alt+A (Shift+Alt+R for long press)
   - footswitch **Fullscreen**
      - will toggle between normal screen and full screen
	  - sends Shift+Alt+F (Shift+Alt+Z for long press)
   - footswitch **Forward**
      - will jump forward 5sec
	  - sends Shift+Alt+D (Shift+Alt+H for long press)
   - footswitch **Speed**
      - will toggle between playback rates (100% -> 75% -> 50% -> 35% -> 20% and back again to 100%)
	  - sends Shift+Alt+S (Shift+Alt+Q for long press)

 - bank 2 is for any other application like your DAW
   - it will send MIDI Program Change (PC) messages on channel 16 that can be assigned to useful functions in your DAW
   - **Play / Pause** sends 5 (55 for long press)
   - **Loop** sends 2 (22 for long press)
   - **Back** sends 4 (44 for long press)
   - **Fullscreen** sends 3 (33 for long press)
   - **Forward** sends 6 (66 for long press)
   - **Speed** sends 1 (11 for long press)
   - if you want to change them...
     - modify the [Arduino code](Arduino/OVLP/OVLP.ino) and upload it to the Arduino Micro
     - instructions where to modify the code are right at the top of that file
