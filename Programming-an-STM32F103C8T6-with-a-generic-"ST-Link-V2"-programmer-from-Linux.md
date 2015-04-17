The following was tested on an Ubuntu Linux (14.10) box, so your mileage may vary if you are using some other platform.

This is an alternative method for programming the STM32F103XXX series devices. 

You will probably find in most cases it easier to use a USB to serial converter or install the maple boot loader and use dfu-util 

This method is a work in progress, so expect these instruction to be incomplete, I am jotting them down before I forget what I did. 

First take the new ST-Link clone and plug it in to one of your USB ports. [These little boards](http://www.ebay.com/itm/ST-Link-V2-Shell-STM8-STM32-Emulator-Download-Programmer-Debugger-Random-Color-/371295635365?pt=LH_DefaultDomain_2&hash=item5672f1c7a5) are very cheap, possibly even cheaper than a usb serial adapter. 

Look in **dmesg** and you should see something like this...

**[ 3377.046983] usb 1-1.5.3.3: USB disconnect, device number 11**

**[ 3439.715490] usb 1-1.5.3.3: new full-speed USB device number 12 using ehci-pci**

**[ 3439.826110] usb 1-1.5.3.3: New USB device found, idVendor=0483, idProduct=3748**

**[ 3439.826120] usb 1-1.5.3.3: New USB device strings: Mfr=1, Product=2, SerialNumber=3**

**[ 3439.826125] usb 1-1.5.3.3: Product: STM32 STLink**

**[ 3439.826129] usb 1-1.5.3.3: Manufacturer: STMicroelectronics**

**[ 3439.826133] usb 1-1.5.3.3: SerialNumber: R\xffffffc3\xffffffbf\xffffffbfo\x06Q\xffffffc2\xffffff88\xffffff88QTQ)"g**

Next, wire the board up to the target. The pins used may be less than obvious, I went for the minimum, and used my Yellow Headers STM32 board, which looks like this...
![Generic STM32F103C8T6](https://lh3.googleusercontent.com/-ohiI0rIgvgY/VODXkdmweZI/AAAAAAAAB8k/B3Ncrwwzgyk/w777-h583-no/RIMG0602.JPG)

.. I used this partly because it didn't have an STM JTAG header on it, and I like to make life difficult for myself, but mainly because the other boards were at home. 

I wired it as per the pictures here, having figured out that the pins I needed for jtag were *probably* on the right angled header... Programmer end wired like this...

![STLink V2 programmer](https://lh4.googleusercontent.com/-C7DNBrlaMTM/VS-2Fj8yVoI/AAAAAAAACCk/Opx0x4sQe5I/w1193-h895-no/IMG_0169.JPG)
_# Yes, the programmer really is that tiny._

... and here is the target board end...

![STLink V2 Board wiring](https://lh4.googleusercontent.com/-NdBz7TJesHk/VS-0rFF-73I/AAAAAAAACCQ/yQzSBV6m3_M/w1193-h895-no/IMG_0170.JPG)

.. and a few more related pictures can be found [here](https://plus.google.com/u/0/photos/+AndrewHull-penguin-droppings/albums/6115348934192197553/6138323441149647122?pid=6138323441149647122&oid=111082960064282217370)...

WARNING: I am powering the STM from the programmer, if you intend to plug in the USB port at the same time, then remove the power (orange) wire first, otherwise you may see some magic smoke.

I had previously installed openocd from the Ubuntu repos, but I like to live dangerously and decided to use the latest version from the GIT repo.

The following recipe did the trick for me....

Remove any old version of openocd

**sudo apt-get remove openocd**

**unset CXX** 

NOTE: This unset command is optional, but perhaps necessary if you have been using cross compilers from the command line. I have been compiling for the Raspbery Pi on my machine, and unless I do this I get the wrong compiler.

Next create a folder to put the openocd git repo in

**mkdir -p ~/sandbox**

**cd ~/sandbox**

**mkdir openocd**

**git clone git://repo.or.cz/openocd.git**

**cd openocd**

**sudo apt-get install make libtool pkg-config autoconf automake texinfo**

**./bootstrap**

**./configure**

**make**

**sudo make install**

Check that openocd runs and if so, we can move on...

Next we need to run openoc, but since I had no clue how this works, I opted to run it in a bash loop using the default config files that ship with openocd....

**while true; do openocd -f ./tcl/interface/stlink-v2.cfg -f ./tcl/target/stm32f1x.cfg; sleep 1;done**

.. I'll come back to this once I have figured out how to do this correctly.

In another terminal window we telnet to the openocd console on port 4444

**telnet 4444**

Now while that is running, we need to do a couple of things in the correct order

Press and hold the reset button on the target....

In the telnet session issue a reset halt command.

**> reset halt **
**timed out while waiting for target halted**
**TARGET: stm32f1x.cpu - Not halted**
**in procedure 'reset'**
**in procedure 'ocd_bouncer'**

... release the reset button.. target should halt after reset...

**target state: halted**
**target halted due to debug-request, current mode: Thread**
**xPSR: 0x01000000 pc: 0x0800016c msp: 0x20005000**
**> **

Bingo... we have control.. 

.. so what's the first thing we need to do... dump the existing firmware...

**dump_image dump.bin 0x08000000 0x1ffff**
**dumped 131071 bytes in 2.825877s (45.295 KiB/s)**
**> **

We have *complete* control, so we can now program the device... so why not flash the maple bootloader on it?

Grab the bootloader for the Maple Mini from here... 

http://static.leaflabs.com/pub/leaflabs/maple-bootloader/maple_boot.bin

Then upload it to the device...

**> reset halt**
**target state: halted**
**target halted due to debug-request, current mode: Thread **
**xPSR: 0x01000000 pc: 0x0800016c msp: 0x20005000**
**> flash write_image erase ~/Downloads/maple_mini_boot.bin  0x08000000**
**auto erase enabled**
**target state: halted**
**target halted due to breakpoint, current mode: Thread **
**xPSR: 0x61000000 pc: 0x2000003a msp: 0x20005000**
**wrote 16384 bytes from file ~/Downloads/maple_mini_boot.bin in 0.949075s (16.859 KiB/s)**
**> reset run**
 
Now if you unplug and replug the device in to a USB port and look in dmesg...

**[95669.294185] usb 1-1.2: new full-speed USB device number 34 using ehci-pci**
**[95669.387630] usb 1-1.2: New USB device found, idVendor=1eaf, idProduct=0003**
**[95669.387634] usb 1-1.2: New USB device strings: Mfr=1, Product=2, SerialNumber=3**
**[95669.387636] usb 1-1.2: Product: Maple 003**
**[95669.387637] usb 1-1.2: Manufacturer: LeafLabs**
**[95669.387638] usb 1-1.2: SerialNumber: LLM 003**
 
... you should see the Maple Leaflabs device enumarated. 

