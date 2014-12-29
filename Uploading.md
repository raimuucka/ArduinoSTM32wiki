## Windows

A number of different methods to upload are available to Windows users. The choice of method is mainly dependent, on the type of board this is being used.  

### Maple and Maple mini

The primary method to upload on the Maple and Maple mini is via the built in USB connection, via the LeafLabs drivers. (Please see details and issues with these drivers on the Maple / Maple mini page).

### Generic / unbranded STM32F103 boards

The easiest way to upload to these board is to use a USB to Serial adapter connected to Hardware Serial 1  (Pins PA9 and PA10).

Additionally to enter serial bootloader mode, these boards, need to be configured so that Boot0 is HIGH and Boot1 is LOW. On most boards these pins are either connected to jump links or to switches.

To upload the board must be reset (with Boot0 HIGH and Boot1 LOW), so that the board enters serial bootloader mode.
Once the upload is complete, the bootloader is instructed to run the code that has just been uploaded.

Note. If the board is reset again, the code will not run if Boot0 is still HIGH etc, so once development is complete, both Boot0 and Boot1 need to be set to LOW so that the code is run immediately after power on or restart.

The additional benefit of using a USB to Serial converter, is that for debugging, Serial.print() etc are sent to the same serial port that is use for uploading, and the whole build and debugging process is very much like using an Arduino Pro Mini board.


### Boards with on-board STLink capability 

STM Nucleo boards come with an onboard STLink, for these boards, the STLink protocol can be used, but currently only upload is supported, not Serial comms via STLink. Hence this is not generally the preferred option even for boards with this capability.


## Linux

Before each upload the board needs to be put into "Perpetual bootloader" mode.

* Press the reset button (it’s the button labeled RESET). The board should blink quickly 6 times, then blinks slowly a few more times, then stop flashing (unless it has a sketch loaded that flashes the LED)
* Press reset again, and this time push and hold the other button during the 6 fast blinks (the normal button is labeled BUT). You can release it once the slow blinks start.

Someone did a Youtube video of this http://youtu.be/rvNIeKuXsxM


## Other operating systems

A Python version of the USB Serial uploader is included in the tools/win folder, and it should be possible to use this to upload under any operating system which supports Python + Python Serial.

