###All OS's

* Download and install the latest stable version of the Arduino IDE from [here](http://arduino.cc/en/Main/Software)
* (1.6.3 or newer) - On the Tools menu, select the Boards manager, and install the Arduino Due from the list of available boards.
* Download zip file containing the STM32 files from [here](https://github.com/rogerclarkmelbourne/Arduino_STM32/archive/master.zip)
* Unzip to create the Arduino_STM32 folder

####Windows 

* Copy the Arduino_STM32 folder to My Documents/Arduino/hardware (note. if the hardware folder doesn't exist you will need to create it)
* If using Maple or Maple mini under windows the Maple Drivers page
* (1.6.2 or newer) - On the Tools menu, select the Boards manager, and install the Arduino Due from the list of available boards.
* Re-start the Arduino IDE, and select the appropriate board from the Tools -> Board menu, and select the appropriate Com port.

####Linux
* Copy the Arduino_STM32 folder to the hardware folder in your Arduino sketches folder . If the hardware folder does not exist, please create one.

* In the ./Arduino_STM32/tools/linux folder set the rights of maple_upload to 755 

* Install DFU-Utils.

* On Ubuntu it has been reported that the standard version of the utils V 0.5 does not work, but V 0.8 does work.
* See the linux page about how to build and install dfu-util

* Note. Please read the uploading section related to uploading when using Linux.

####Mac OSX
* Arduino_STM32 folder must be place inside ~/Documents/Arduino/hardware (note. if the hardware folder doesn't exist you will need to create it)
* So you should get ~/Documents/Arduino/hardware/Arduino_STM32

* Install DFU Utils. The easiest way to do this is to use Homebrew  http://brew.sh/ - see the bottom of the Homebrew page.

* Then type "brew install dfu-util" in the terminal window to install

* See also http://dfu-util.sourceforge.net/

* Note. The if you install DFU Utils using another method than Homebrew the installed location may be different, and you may need to edit the scripts in the tools/osx folder

