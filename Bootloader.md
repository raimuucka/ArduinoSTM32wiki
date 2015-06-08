### A modified "maple" bootloader has been developed as an effort to increase the Flash and RAM available for sketches.
### Please note that changing the bootloader may render the board unusable unless you can re-flash the bootloader by other means (ST-Link or perpetual bootloader mode with Serial connection to UART1, not Serial USB). Change the bootloader only if you are confident with the process.

The details of the new bootloader are :
* Maintains compatibility with uploading from the Maple IDE (for upload to Flash only)
* Increases the RAM available to sketches to the full 20KB in the Maple Mini.
* Increads the Flash available for sketches to 120KB, reserving only 8KB for the bootloader.

To try the new bootloader follow the steps below. To learn all the details, go to the Arduino forum thread to learn about the changes needed.
Please note this is still a beta version and you may need to upload the bootloader again if bugs are found.

Some notes:
The Maple Mini original bootloader supports 2 upload modes, selected by the uploader program with a parameter called Upload ID.
Upload ID=0 uploads to RAM. (This option has been modified so that it returns an error if used - more details below)
Upload ID=1 uploads to Flash. It uploads sketches to 0x8005000, thus reserving the initial 20KB of the flash memory for itself. It also reserves the initial 3KB of ram, so sketches can use up to 17KB.

We added a new upload mode.

Upload ID=2 uploads to Flash. It uploads sketches to 0x8002000, thus reserving the initial 8KB of the flash memory for itself. The bootloader no longer operates with upload to RAM, and hence all RAM in the process is always available to the sketch.

To make use of the new bootloader reduce footprint etc, you need to use an up to date version of the repo, as it has additional menu options for the Maple mini and also for generic boards.

On the Maple mini there is a bootloader menu, which lets you select the original or new bootloader.
On generic boards, the new bootloader (called the stm32duino bootloader) is available as an upload option. This option always uses Upload ID = 2 (see above)


## Removal for Upload to RAM
There were a number of issues with using upload to RAM, and although its ID has been retained so that uploads to ram don't crash the IDE etc, it no longer does anything, and just returns and error to the uploader.
Upload to RAM on Maple Mini and Maple boards was always almost completely useless, as the Maple mini only has 20k RAM, and most usable sketches take at least 20k. However the main reason this has been disabled, is that there were issues with the bootloader being able to determine after a warm boot, whether it should run code in RAM or Flash. The original code, relied on looking in the RAM for markers that suggested that the data at that location was a program, however this wasn't that reliable. Although it would be possible to use the backup registers (non volatile after soft boot) to store data about program start location, it was felt that as the RAM was so limited, it wasn't worth the hassle of rebuilding the RAM upload option to use backup ram as a flag) - and the sketch code could still overwrite the backup registers and break this option.


# How to install the new bootloader:

Currently the only way to install the new bootloader is to flash your board using USB to Serial or STLink (or another SWD device e.g. JTAG or Black Magic Probe etc).

Firstly you need to download the Bootloader that matches your board See

https://github.com/rogerclarkmelbourne/STM32duino-bootloader/tree/master/STM32F1/binaries
e.g. for Maple mini, the bootloader is maple_mini_boot20.bin

If you have a generic board, not a Maple or Maple mini see the section in the wiki about the use of the stm32duino bootloader on generic boards


# How to revert to the original bootloader:

Download the original Maple mini bootloader from the leaflabs site and re-flash using USB to Serial, or STLink etc.



