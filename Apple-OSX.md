Some notes about use with OSX

tools/macosx now contains dfu-util so there is now need to use Homebrew etc to install it

If you are uploading via a USB to Serial adaptor, you will need to load the appropriate driver for your adaptor, and select that device.
There have been reports of some USB to Serial adaptors not working correctly with some versions of OSX - this seems to be a driver issue with FDTI based devices - so please check you have the latest drivers if you encounter this issue.
Also note, that some USB to Serial devices appear with names starting with "tty", the upload reset utility does not work with tty devices. Please select the "cu" device for your board rather than the tty device

STLink does not seem to work with all boards. This appears to be an issue with Texane-stlink (open source stlink driver).
Unfortunately STM don't support STLink uploads on OSX, so if your board does not work with STLink, I'm afraid there is not a lot you can do, except perhaps contact the Texane-stlink dev guys.
 
