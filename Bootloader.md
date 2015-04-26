### A modified bootloader is being developed as an effort to increase the Flash and RAM available for sketches.

Currently the changes are still being tested, but the targets are:
* Maintain compatibility with older bootloader.
* Increase the RAM available to sketches to the full 20KB in the Maple Mini. This change depends on the above.
* Increade the Flash available for sketches to 120KB, reserving only 8KB for the bootloader.

To test the new bootloader follow the steps below. To learn all the details, go to the Arduino forum thread to learn about the changes needed.
Please note this is still a beta version and you may need to upload the bootloader again if bugs are found.

### Current status:
The latest version of the uploader takes exactly 7000 bytes.
We have added a new upload id (id=2), which load the sketch to 0x8002000.
To use the extra flash available, you need the following:
* Latest version of the uploader
* A board definition file that uses the new id=2, new flash maximum, and new ram maximum:
      ...upload.altID=2
      ...upload.ram.maximum_size=20480
      ...upload.flash.maximum_size=122880
* A new linker script or a modified one, with the new starting address for the sketches and the new maximum RAM:
MEMORY
{
  ram (rwx) : ORIGIN = 0x20000000, LENGTH = 20K
  rom (rx)  : ORIGIN = 0x08002000, LENGTH = 120K
}

# How to get to the new bootloader installed:

There are several possible situations, first using a sketch:
### You have a Maple mini with the original bootloader:
       Download this files, and upload the sketch to your Maple mini:
         https://github.com/victorpv/Arduino_STM32/tree/master/maple_mini_bootloader
Once you install the sketch to the Maple mini, open the Serial monitor, it will provide additional information in through the USB serial. You will need to confirm that you want to overwrite the existing bootloader, and if everything goes fine it will let you know it is finished and you can reboot. The update is almost instantaneous.
After you have uploaded, you need to upload your boards.txt definition, and your linker script. If you don't know how to update those manually, upload the latest version of this full repository, which include the updates.

2. You have a Maple Mini with one of the beta versions that only included the option to upload to 8002000, and did not add the new upload id 2:
 * In that case it is likely that you already have changed you boards.txt file and linker script for the new address. If you have not, then no sketch will run.
 * Download the sketch from step 1, and update by running that sketch.
 * After you upload the bootloader, the upload ID=1 is exactly like the original bootloader (108KB available, ROM starts at 0x8005000), and a new ID=2 has been created for uploads to 0x8002000, so you need to either download the latest full repo, to have both options in the Arduino IDE, or at least edit your boards.txt file and linker script as described above.
 * You specially need to be sure that for a menu option using the new flash and ram, and a linker script using flash at 8002000 your Upload ID is 2, and not 1.

### If you are uploading with ST-Link:

Then use the bin file from:
     https://github.com/victorpv/Arduino_STM32/tree/master/maple_mini_bootloader

 * After you load that uploader, the upload ID=1 is exactly like the original bootloader (108KB available, ROM starts at 0x8005000), and a new ID=2 has been created for uploads to 0x8002000, so you need to either download the latest full repo, to have both options in the Arduino IDE, or at least edit your boards.txt file and linker script as described above.
 * You specially need to be sure that for a menu option using the new flash and ram, and a linker script using flash at 8002000 your Upload ID is 2, and not 1.




