####Windows 

* Download and install the latest stable version of the Arduino Beta from [here](http://downloads.arduino.cc/arduino-1.6.0rc1-windows.exe)
* Download zip file containing the STM32 files from [here](https://github.com/rogerclarkmelbourne/Arduino_STM32/archive/master.zip)
* Unzip to create the Arduino_STM32 folder
* Copy the Arduino_STM32 folder to My Documents/Arduino/hardware (note. if the hardware folder doesn't exist you will need to create it)
* If using Maple or Maple mini under windows see below
* Start the Arduino IDE, and select the appropriate board from the Tools -> Board menu, and select the appropriate Com port.

##### Maple and Maple mini windows drivers

If using Maple or Maple mini, you will need to install the drivers in drivers/win folder. Maple and Maple mini use drivers which come as standard on Windows 7 or newer, however USB VID/PID numbers of the Maple boards need to be associated with the relevant drivers, and Windows 7 and newer also require that the drivers and inf files need to be "signed". This is a complex process, but has been made seamless thanks to the work of @timschuerewegen who created a modified version of dwi-simple (To Do . link to DWI)
Run the install_drivers.bat file, which runs dwi-simple.exe twice, once to install the usb serial device and once to install the DFU device. 

####Linux
Download and install from either

Linux 32:  http://downloads.arduino.cc/arduino-1.6.0rc1-linux32.tgz

or 
Linux 64: http://downloads.arduino.cc/arduino-1.6.0rc1-linux64.tgz 

in the ./Arduino_STM32/STMF1XX/tools/linux folder set the rights of maple_upload to 755 

Install DFU-Utils 

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

