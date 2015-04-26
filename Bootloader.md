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

## How to get to the new bootloader installed:
####

