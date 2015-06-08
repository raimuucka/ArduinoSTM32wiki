The STM32duino bootloader, is an experimental bootloader, based on the Maple bootloader (developed by LeafLabs), however it also works with most (but not all) "Generic" STM32 boards.

Credit also goes to @victor_pv for writing and testing the initial code to use PA12 to reset the USB on generic boards.

#####Generic vs Maple boards
"Maple" boards e.g. the Maple mini (and clones) and the Maple (and Maple clones) have hardware (2 transistors and several resistors), which is attached to the USB bus, and which the software uses to trigger a USB reset and re-enumeration.
Generic board (non-Maple boards), don't have this additional hardware, however they normally have a USB connection.

Installable binaries for the stm32duino bootloader can be found at 
https://github.com/rogerclarkmelbourne/STM32duino-bootloader/tree/master/STM32F1/binaries

There are 2 main versions of the bootloader, the versions starting with the word "maple" operate with the Maple USB reset hardware, and the versions with starting with the word "generic" use GPIO to control PA12 (USB D+) in order to reset the USB bus.

Within the generic bootloaders are different versions depending on the location of the LED on the generic board e.g. generic_boot20_pc13.bin  is suitable for a generic board with an LED on pin PC13.

Its not a requirement that the generic board has a LED pin that can be controlled by the bootloader and people have reported using the generic bin file on boards which dont have an LED, however its much easier to know if the upload is working if the board has a LED and the appropriate bootloader has been installed.

Notes

(1) The bootloader automatically senses the size of flash in the device onto which its loaded and infers the flash page size (1k for devices up to 128k flash, and 2k page size for > 128k flash size0

(2) You can optionally fit a button which will force the bootloader into "perpetual bootloader mode". This is the leaflabs name for the bootloader being forced into waiting for a DFU upload, rather then existing the bootloder 1/2 second after startup, and running the user code (at 0x8002000).
The Button needs to be on pin PC14.
If you have something attached to PC14 which will pull this pin high at boot time, you may need to rebuild the bootloader to change the pin for the Button, or remove the button functionality from the bootloader altogether.


#####Installing the stm32duino bootloader.

The bootloader bin needs to be "flashed" onto generic boards as they usually do not contain any form of bootloader. 

The simplest method to flash the bootloader onto a generic board is to use a USB to Serial adpator attached to Serial 1 PA9 and PA10, and set Boot0 HIGH.

On Windows. Open a command prompt and CD to the repo install directory, in My Documents\Ardruino\hardware\Arduino_STM32\tools\win (Other OS's use the same structure below Arduino_STM32).

Then run the exe

stm32flash.exe -w ..\..\STM32duino-bootloader\STM32F1\binaries\generic_boot20_pc13.bin COM_XXXX

Where COM_XX is the windows com port e.g. COM14 of your USB to serial device, 
If your board has the LED on a different pin, use the bin file that matches your board.

After the board has been flashed, you can remove the USB to Serial adaptor, set Boot0 to Low, and connect via USB. 
However prior to connecting on Windows, you should also install the windows "driver" stub files, (see /Arduino_ST32/driver/win batch file)

After the drivers are installed... When you connect your generic board; Windows should initially recognise it as a libUSB Maple DFU device, the LED should flash quickly 6 times, After this the bootloader will continue to flash the LED at a slighly slower rate. The slower flashing indicates the bootloader has not found an existing sketch and is waiting for an upload. In the Windows device manager you should see a libUSB Maple DFU device.

In the Arduino IDE, select your generic board from the menu, e.g. Generic STM32F103C series, then select Stm32duino bootloader from the upload menu.


If your board doesn't appear on USB at all, I'm afraid either its not supported or its faulty.
Some cheap boards seem to lack even the most rudimentary usb hardware e.g. the necessary pullup resistors; so unless you are handy with a soldering iron, these boards will probably never work with the bootloader.





