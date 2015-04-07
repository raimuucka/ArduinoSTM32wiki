Details from @ahull on the Arduino forum on how its possible to install and use the Maple bootloader on a generic STM32 board (one that doesn't have the Maple USB reset hardware) on Linux

(Apologies for the text formatting in this page. but I wanted to get it into the wiki before it got lost)

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



and restart udev


```

sudo /etc/init.d/udev restart 



(or whatever is appropriate for your Linux version)

Next you need to ensure that upload_router is functional. At the time of writing, there was no linux version of upload router in the git repo, so if this is the case, save the following to tools/linux/upload_router and symlink this to tools/win/upload_router (to ensure that the IDE calls the linux version).



```
#!/bin/bash
# Translates the windows Arduino IDE upload call - something like..
#
# upload_router ttyUSB0 1 1EAF:0003 /tmp/build9114565021046468906.tmp/STM32_Blink.cpp maple_dfu 0
#
# to the linux equivalent of the form...
#
# dfu-util -D ./STM32_Blink.cpp.bin -d 1eaf:0003 --intf 0 --alt 1
#
# Lowercase the 1eaf device name, since in Windows land everybody shouts.
#
DEVICE="$3"
DEVICE=${DEVICE,,}
BINFILE="$4.bin"
INTERFACE="$6"
ALT_INTERFACE="$2"
USBRESET=$(which usb-reset) || USBRESET="./usb-reset"
echo -e "\n\rPlease unplug and replug the USB cable to the STB device..."
sleep 2
while [[  $(lsusb |grep -i "1eaf") ]]
do
    echo -n "."
    sleep 1
done

until dfu-util -v -D "$BINFILE" -d "$DEVICE" --intf "$INTERFACE" --alt "$ALT_INTERFACE"
do
    echo -n "."
    sleep 1
done

echo -e "\n\rUnplug and replug the USB cable to the STM board again please...."
while [[  $(lsusb |grep -i "1eaf\:0003") ]]
do
    echo -n "."
    sleep 1
done

echo -e "\n\rReconnecting"
while [ -z "$(lsusb |grep -i "1eaf\:0003")" ]
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
for i in {1..4}
do
    echo -n "."
    sleep 1
done

echo -e "\n\rSerial port re-created... $(ls "/dev/ttyACM"*)\n\r"
for i in {1..4}
do
    echo -n "."
    sleep 1
done

```
Next we need a method to reset the device once we have programmed it. We use the code from here http://askubuntu.com/questions/645/how-do-you-reset-a-usb-device-from-the-command-line. Copy the code below and save as usb-reset.c

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


Compile with ...  

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

Once in a while the IDE fails to connect the serial monitor after running the above upload_router. It almost invariably works on the 2nd  (or worst case 3rd) attempt at clicking "Serial Monitor" in the IDE. Some sort of race condition I suspect. Other than that minor flaw, it works.
