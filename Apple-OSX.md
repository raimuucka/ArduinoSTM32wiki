Just some quite notes about use with OSX

If you are using a Maple or Maple mini board..

1. You will need to install the DFU driver. See the installation section on how to do this
2. Serial should work OK without any additional drivers, and will appear as tty.usbModemxxxxx (where xxxxx seems to vary)

There seem to be issues with the IDE no longer resetting the board via the Serial interface, so its likely that you board will need to be put into "perpetual bootloader" mode by resetting the board and pressing the button while the led is blinking quickly. The and then releasing the "button" as soon as the led begins to blink more slowly.


If you are uploading via a USB to Serial adaptor, you will need to load the appropriate driver for your adaptor

Uploading via STlink uses texane-stlink which doesn't appear to require a driver.

