The Maple and Maple mini were originally developed my [Leaflabs](http://www.leaflabs.com/about-maple/)

Details of the original Maple can be found [here](http://leaflabs.com/docs/hardware/maple.html)
Details of the original Maple mini can be found [here](http://leaflabs.com/docs/hardware/maple-mini.html)

As the hardware designs for these boards is open source and available on the [GitHub Leaflabs wiki](https://github.com/leaflabs) , clones of the Maple mini are now being mass produced in China and are available through multiple suppliers on [eBay](http://www.ebay.com/sch/i.html?_odkw=maple+mini+clone&_from=R40|R40&_osacat=0&_from=R40&_nkw=maple+mini+stym32&_sacat=0) and [AliExpress](www.aliexpress.com/wholesale?SearchText=maple+mini+stm32)


The Maple and Maple mini differ from other generic aka "minimum development" boards, as they have additional electronics connected to the USB connector. This additional hardware and the use of a custom bootloader, (developed my Leaflabs), allow the USB port to be used as a virtual Serial device and also allow uploads via USB (DFU)

To use the Maple mini (including clones), windows drivers for both the USB Serial and DFU upload need to be installed.  However there is a problem with this on Windows 7 onwards, because the drivers are not digitally signed and Windows will not install them.

The work-around is to disable "Device driver signing enforcement", however this does cause a slight security risk. There are various methods to do this [See](https://social.technet.microsoft.com/Forums/windows/en-US/9b6eee60-855d-47cc-9927-acae3fb6f971/permanently-disable-driver-signature-enforcement-on-win-7-x64?forum=w7itprohardware)


####Changes from the original version of Maple IDE

Some changes have been made to the API to make everything more compatible with the Arduino API

* SerialUSB is now just called Serial
* HardwareSPI is now called SPI and does not need to be instantiated as part of the sketch, as its instantiated as part of the SPI class when it is included via #include <SPI.h>
