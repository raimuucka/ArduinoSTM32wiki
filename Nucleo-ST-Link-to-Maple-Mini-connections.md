This great short guide was posted by Madias in the forum:
(Quick and dirty) upload Bootloader on a maple mini with any nucleo board (via ST-Link 2.1)
Disconnect both CN2 jumper (left bottom) on nucleo
SWD connector:
Pin1: VCC (not working for me? Take any VCC pin and connect it to "Vin" on the mini)
Pin2: SWCLK -> D21 on mini
PIn3: GND -> GND
Pin4: SWDIO -> D22 on mini
Pin5: NRST -> RST on mini
Pin6: SWO (nothing connected)
Get your maple mini into the serial bootloader mode: Hold both buttons (reset + button) and then release the "reset" button (maybe not needed if maple isn't freezed)
now drag&drop any *.bin file (bootloader) in your finder/explorer to upload it.