##Windows

If using Maple or Maple mini, you will need to install the drivers in drivers/win folder.

Maple and Maple mini use drivers which come as standard on Windows 7 or newer, however USB VID/PID numbers of the Maple boards need to be associated with the relevant drivers, and Windows 7 and newer also require that the drivers and inf files need to be "signed". 

This is a complex process, but has been made seamless thanks to the work of @timschuerewegen who created a modified version of dwi-simple (See https://github.com/pbatard/libwdi. I will add the modified sources to the repo when I get around to it.)

Run the install_drivers.bat file, which runs dwi-simple.exe twice, once to install the usb serial device and once to install the DFU device. 

