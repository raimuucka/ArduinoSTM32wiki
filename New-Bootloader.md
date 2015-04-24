# A modified bootloader is being developed as an effort to increase the Flash and RAM available for sketches.

Currently the changes are still being tested, but the targets are:
* Remove upload sketch to RAM in the Maple Mini. Even an empty sketch takes more space than the RAM available, so that option was not usable for a long time, and caused the bootloader to reserve 3KB of RAM, not available for any sketch even running from flash.
* Increase the RAM available to sketches to the full 20KB in the Maple Mini. This change depends on the above.
* Increade the Flash available for sketches to 120KB, reserving only 8KB for the bootloader. This change is independent of the two above changes, and seems to be already successful by changing compile time optimizations.

To test the new bootloader go to the Arduino forum thread to learn about the changes needed.
Once everything is tested the procedure for updating the Maple Mini bootloader will be described in this page.
It can be done by uploading a sketch to the board like any other sketch, so it does not require ST-Link, perpetual bootloader mode, or any other special uploading mode.

