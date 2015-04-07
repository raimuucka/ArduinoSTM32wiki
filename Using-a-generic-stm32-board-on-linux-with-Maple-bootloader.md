Details from @ahull on the Arduino forum on how its possible to install and use the Maple bootloader on a generic STM32 board (one that doesn't have the Maple USB reset hardware) on Linux

Using the maple bootloader under Linux to allow access to the USB serial port on generic STM32 boards with the maple bootloader installed to them (should also work with genuine Maple boards).

Obviously the principle requirements are a Linux PC and a Maple board, or a generic board with the Maple bootloader installed, USB cable etc.

In addition to the requirements for installing the Arduino IDE and so forth as detailed here...

https://github.com/rogerclarkmelbourne/Arduino_STM32/wiki/Installation

... you will need ...

dfu-util

Either install the packaged version for your particular Linux (sudo apt-get install dfu-util for Debian based versions including Ubuntu etc) or install from...

http://dfu-util.sourceforge.net/


Some additional udev rules are needed to ensure that the maple 1eaf devices are correctly enumerated.

See your Linux documentation for details. The rules below will work for most Linux versions.

```
ATTRS{idProduct}=="1001", ATTRS{idVendor}=="0110", MODE="664", GROUP="plugdev"
ATTRS{idProduct}=="1002", ATTRS{idVendor}=="0110", MODE="664", GROUP="plugdev"
ATTRS{idProduct}=="0003", ATTRS{idVendor}=="1eaf", MODE="664", GROUP="plugdev" SYMLINK+="maple"
ATTRS{idProduct}=="0004", ATTRS{idVendor}=="1eaf", MODE="664", GROUP="plugdev" SYMLINK+="maple"
```

Save the rules locally as 45-maple.rules then do something like...

```
    sudo cp -v 45-maple.rules /etc/udev/rules.d/45-maple.rules
    sudo chown root:root /etc/udev/rules.d/45-maple.rules
    sudo chmod 644 /etc/udev/rules.d/45-maple.rules
```

and restart udev

sudo /etc/init.d/udev restart 

(or whatever is appropriate for your Linux version)

Next you need to ensure that upload_router is functional. At the time this wiki page was first written, there was no linux version of upload router in the git repo.

There should be a more up to date version of the upload_router included with the latest code, but the version below will let you see how this works. 

The upload_router script relies on lsusb, dfu-util so if those dependencies are not installed on your system, install them first.  

```
#!/bin/bash
# Translates the windows Arduino IDE upload call - something like..
#
# upload_router ttyUSB0 1 1EAF:0003 /tmp/build9114565021046468906.tmp/STM32_Blink.cpp maple_dfu 0
#
# to the linux dfu-util equivalent of the form...
#
# dfu-util -D ./STM32_Blink.cpp.bin -d 1eaf:0003 --intf 0 --alt 1
#
#

function leaf_status() 
{

this_leaf_status=$(lsusb |grep "1eaf" | awk '{ print $NF}')
# Find the mode of the leaf bootloader
case $this_leaf_status in 
   "1eaf:0003")
      echo "dfu"
   ;;
   "1eaf:0004")
      echo "ttyACMx"
   ;;
   *)
      #echo "$this_leaf_status"
      echo "unknown"
   ;;
esac
}


DEVICE="$3"
# Lowercase the 1eaf device name, since in Windows land everybody shouts.
DEVICE=${DEVICE,,}

BINFILE="$4.bin"
INTERFACE="$6"
ALT_INTERFACE="$2"

# You will need the usb-reset code, see https://github.com/rogerclarkmelbourne/Arduino_STM32/wiki/Using-a-generic-stm32-board-on-linux-with-Maple-bootloader
#
USBRESET=$(which usb-reset) || USBRESET="./usb-reset"

# Check to see if a maple compatible board is attached
LEAF_STATUS=$(leaf_status)

# Borard not found, or no boot loader on board.
if [[ $(leaf_status) = "unknown" ]]
then
   echo "STM32 Maple Bootloader compatible board not found."
   sleep 5
   exit 1
fi

# We got this far, so we need to get the board in bootloader mode. 
# After the timeout period, the board goes back in to serial mode, we need it in dfu mode, which happens for the first few seconds at power on 
# so we ask the user to unplug and re-plug the board. 
echo -e "\n\rSTM32 Maple board is in $LEAF_STATUS mode."

echo "Please unplug and replug the USB cable to the Maple device."
sleep 2
# On unplugging the board will be "unknown"
while [[ $(leaf_status) != "unknown" ]]
do
   echo -n "."
   sleep 1
done
# On re-plugging the board will be "dfu"
while [[ $(leaf_status) != "dfu" ]]
do
   echo -n "."
   sleep 1
done

echo -e "\n\rProgramming STM32 device with dfu-util"
until dfu-util  -D "$BINFILE" -d "$DEVICE" --intf "$INTERFACE" --alt "$ALT_INTERFACE" 2>&1
do
    echo -n "."
    sleep 1
done

echo -e "\n\rUnplug and replug the USB cable to the STM32 board again please...."
while [[  $(leaf_status) != "unknown" ]]
do
    echo -n "."
    sleep 1
done

echo -e "\n\rReconnecting"
while [[ $(leaf_status) = "unknown" ]]
do
    echo -n "."
    sleep 1
done

echo -e "\n\rWaiting for bootloader to exit."
for i in {1..6}
do
    echo -n "."
    sleep 1
done

"$USBRESET" "/dev/bus/usb/$(lsusb |grep "1eaf" |awk '{print $2,$4}'|sed 's/\://g'|sed 's/ /\//g')" >/dev/null 2>&1

while [[ $(leaf_status) = "unknown" ]]
do
    echo -n "."
    sleep 1
done
THIS_TTY=$(find /dev/ttyACM* -cmin -2)
echo -e "\n\rSTM32 Maple board serial port re-created..."
echo -e "\n\rSerial port is $THIS_TTY Please allow 15 seconds before attempting to read from serial port."

```
Next we need a method to reset the device node in linux once we have programmed the board, in order to correctly enumerate the maple serial device as a tty, otherwise linux still thinks it has a dfu device attached.  

We use the code from here http://askubuntu.com/questions/645/how-do-you-reset-a-usb-device-from-the-command-line. 

Copy the 'c' code below and save as usb-reset.c

```
/* usb-reset -- send a USB port reset to a USB device
 
    Compile with ...  
    gcc usb-reset.c -o usb-reset
    ... then copy the resulting usb-reset binary to /usr/bin or some other suitable place in your PATH

*/

#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <errno.h>
#include <sys/ioctl.h>

#include <linux/usbdevice_fs.h>


int main(int argc, char **argv)
{
    const char *filename;
    int fd;
    int rc;

    if (argc != 2) {
        fprintf(stderr, "Usage: usbreset device-filename\n");
        return 1;
    }
    filename = argv[1];

    fd = open(filename, O_WRONLY);
    if (fd < 0) {
        perror("Error opening output file");
        return 1;
    }

    printf("Resetting USB device %s\n", filename);
    rc = ioctl(fd, USBDEVFS_RESET, 0);
    if (rc < 0) {
        perror("Error in ioctl");
        return 1;
    }
    printf("Reset successful\n");

    close(fd);
    return 0;
}

```
Compile the 'c' with ...  

  gcc usb-reset.c -o usb-reset

... then copy the resulting usb-reset binary to /usr/bin or some other suitable place in your PATH

The work flow now is pretty much as described in the bash script.

Plug in your board with Maple Bootloader, select Maple Mini Generic as the board type and compile your sketch to prove it compiles, When you are happy that this all works, press the Upload button in the IDE. This should start upload_router if you have followed the above instructions without any problems.

1) upload_router prompts you to unplug the board from the USB port, and re-plug it.
upload_router then uses dfu-util to program the board.

2) upload_router then prompts to unplug and replug the board again.
usb-reset then identifies the leaf (1EAF) device and resets it using the usb-reset command (from the c code above).

3) upload_router then waits while linux re-enumerated the device.
/dev/ttyACM0 or /dev/ttyACM{whatever} automagically appears and if we look in lsusb, the device has changed from something like...

Bus 001 Device 114: ID 1eaf:0003

.. to ..

Bus 001 Device 115: ID 1eaf:0004

You should be able to use the USB serial port and the serial monitor in the IDE if you set serial.port=/dev/ttyACM1 (or serial.port=/dev/ttyACM2 or whatever the board enumerates as) in your IDE preferences.txt

If you are afraid of wearing out your laptop usb port with all that unplugging, use a USB extension lead.

Once in a while the IDE fails to connect the serial monitor after running the above upload_router. 
It almost invariably works on the 2nd  (or worst case 3rd) attempt at clicking "Serial Monitor" in the IDE. 
Other than that minor flaw, it works. 

This looks to be some sort of timeout in the maple bootloader, so allow 15 seconds once the board has rebooted for the serial port to come back up. 

I took another quick look at trying to reset the STM32 USB device from software, but the problem is that I need to cut the USB power and there seems to be no reliable programatic method to do this. Although a lot of USB chipsets include this feature, most hardware vendors don't include the necessary circuitry to actually implement it. 

I think we are stuck with physical reset. i.e. Physically cut the USB power and reconnect , for non Maple boards.

I tried toggling DTR and echoing "1eaf" to the device, but it just ignored me. Even pressing the reset button on the STM32 board doesn't make a difference, only powering the board off and back on lets Linux know it has changed state. 

Perhaps I might take another look if I get the time, but to be honest it would be quicker to wire up a make/break switch on the +5V line in a USB extension lead to do the same thing if damaging your USB ports is a concern. 

Just [url=http://www.ebay.com/itm/USB-Power-Cable-with-ON-OFF-Switch-Power-Control-for-Raspbeery-Pi-Arduino-Phone-/261837515315?pt=LH_DefaultDomain_0&hash=item3cf6bb6a33]make something like one of these[/url] and (assuming you have used 4 core wire), wire the data lines through... or build you own solution from whatever you have to hand. 

Alternatively, [url=http://www.ebay.com/itm/4-Ports-USB-2-0-Hub-High-Speed-Up-to-480Mbps-With-Power-On-Off-Button-Switch-Led-/201259337620?pt=LH_DefaultDomain_0&hash=item2edbfdc794]one of these[/url] or if you have lots of devices to program, [url=http://www.ebay.com/itm/New-Hot-7-Port-USB-2-0-Hub-High-Speed-ON-OFF-Sharing-Switch-For-PC-Laptop-/330989851927?pt=LH_DefaultDomain_2&var=&hash=item4d10885517]one of these, but you would need to externally power it[/url], will do the trick.