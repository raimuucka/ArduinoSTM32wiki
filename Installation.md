####Windows 

* Download and install the latest stable version of the Arduino Beta from [here](http://downloads.arduino.cc/arduino-1.6.0rc1-windows.exe)
* Download zip file containing the STM32 files from [here](https://github.com/rogerclarkmelbourne/Arduino_STM32/archive/master.zip)
* Unzip to create the Arduino_STM32 folder
* Copy the Arduino_STM32 folder to My Documents/Arduino/hardware (note. if the hardware folder doesn't exist you will need to create it)
* If using Maple or Maple mini under windows the Maple Drivers page
* Start the Arduino IDE, and select the appropriate board from the Tools -> Board menu, and select the appropriate Com port.

####Linux
Download and install from either

Linux 32:  http://downloads.arduino.cc/arduino-1.6.0rc1-linux32.tgz

or 
Linux 64: http://downloads.arduino.cc/arduino-1.6.0rc1-linux64.tgz 

in the ./Arduino_STM32/tools/linux folder set the rights of maple_upload to 755 

Install DFU-Utils.

On Ubuntu it has been reported that the standard version of the utils V 0.5 does not work, but V 0.8 does work.
See the linux page about how to build and install dfu-util

Note. Please read the uploading section related to uploading when using Linux.

####Mac OSX

Download from  http://downloads.arduino.cc/arduino-1.6.0rc1-macosx.zip 

Install DFU Utils. The easiest way to do this is to use Homebrew  http://brew.sh/ - see the bottom of the Homebrew page.

Then type "brew install dfu-util" in the terminal window to install

See also http://dfu-util.sourceforge.net/

Note. The if you install DFU Utils using another method than Homebrew the installed location may be different, and you may need to edit the scripts in the tools/osx folder


####Other operating systems see download links here

[https://groups.google.com/a/arduino.cc/forum/#!topic/developers/2_GD40Sl6FA](https://groups.google.com/a/arduino.cc/forum/#!topic/developers/2_GD40Sl6FA)

Then follow the windows instructions

