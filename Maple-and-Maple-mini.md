The Maple and Maple mini were originally developed my [Leaflabs](http://www.leaflabs.com/about-maple/)

Details of the original Maple can be found [here](http://leaflabs.com/docs/hardware/maple.html)
Details of the original Maple mini can be found [here](http://leaflabs.com/docs/hardware/maple-mini.html)

As the hardware designs for these boards is open source and available on the [GitHub Leaflabs wiki](https://github.com/leaflabs) , clones of the Maple mini are now being mass produced in China and are available through multiple suppliers on [eBay](http://www.ebay.com/sch/i.html?_odkw=maple+mini+clone&_from=R40|R40&_osacat=0&_from=R40&_nkw=maple+mini+stym32&_sacat=0) and [AliExpress](www.aliexpress.com/wholesale?SearchText=maple+mini+stm32)


The Maple and Maple mini differ from other generic aka "minimum development" boards, as they have additional electronics connected to the USB connector. This additional hardware and the use of a custom bootloader, (developed my Leaflabs), allow the USB port to be used as a virtual Serial device and also allow uploads via USB (DFU)

To use the Maple mini (including clones), windows drivers for both the USB Serial and DFU upload need to be installed.  See the installation page for how to install the drivers for Windows and the Linux usb device installation script. (Note OSX doesn't need any installation of drivers etc)


####Changes from the original version of Maple IDE

Some changes have been made to the API to make everything more compatible with the Arduino API

* SerialUSB is now just called Serial
* HardwareSPI is now called SPI and does not need to be instantiated as part of the sketch, as its instantiated as part of the SPI class when it is included via #include <SPI.h>
* Referenced to the BOARD LED and the BUTTON have been removed from the core headers because they only apply to the Maple and Maple mini 
