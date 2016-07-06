###All OS's

* Download and install the version 1.6.9 of the Arduino IDE from [here](http://arduino.cc/en/Main/Software)
* Run the IDE, and on the Tools menu, select the Boards manager, and install the Arduino Zero from the list of available boards. You must do this step, it installs the arm-none-eabi-g++ toolchain!
* Download zip file containing the STM32 files from [here](https://github.com/rogerclarkmelbourne/Arduino_STM32/archive/master.zip)
* Unzip to create the Arduino_STM32 folder

####Windows 

* Copy the Arduino_STM32 folder to My Documents/Arduino/hardware (note. if the hardware folder doesn't exist you will need to create it)

* If using Maple or Maple mini, you need to install drivers for the Serial and DFU (upload devices). Please run drivers/win/install_drivers.bat 
Note. This doesn't actually install drivers. Windows comes pre-installed with a compatible Serial USB driver and a DFU (upload) driver. However the built in drivers need to be associated with the USB ID of the Maple serial and DFU devices. The batch file and wdi-simple.exe do the clever stuff to convince Windows 7 or newer, that it should use its drivers with the Maple serial and DFU devices(Run install_drivers.bat in the "drivers/win" folder).

* Re-start the Arduino IDE, and select the appropriate board from the Tools -> Board menu, and select the appropriate Com port for your Maple mini or serial upload device.
Note. If you do not see a Maple Serial com device, this is probably because the Maple mini has not been loaded with the blink sketch. So upload a the Maple mini blink sketch from examples\Digital\Blink and the Maple serial device should now be available on the Port menu.

####Linux

* Copy the Arduino_STM32 folder to the hardware folder in your Arduino sketches folder . If the hardware folder does not exist, please create one.
* Run the udev rulues installation script in tools/linux/install.sh
* Note. If you are uploading via USB to Serial or STLink etc, you may need to set the relevant permissions for your specific upload device in order to be able to use it from within the Arduino IDE. You may also need to change the udev rules for the device in question.

####Mac OSX
* Arduino_STM32 folder must be placed inside ~/Documents/Arduino/hardware (note. if the hardware folder doesn't exist you will need to create it)
* So you should get ~/Documents/Arduino/hardware/Arduino_STM32
* Note. DFU util binaries have now been added to the repo, in tools/macosx/dfu-util, so there is no longer a need to install Homebrew to then install dfu-util, or to compile dfu-util from source.
