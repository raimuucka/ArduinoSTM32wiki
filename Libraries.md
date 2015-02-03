Currently the libraries that come with the IDE work, (with some caveats), However very few third party libraries have been tested, and its likely that many third party Libraries don't currently work (or even compile).

####Built-in and Maple specific libraries

* SPI. Partially working. But lacks full Transaction support. Transactional SPI is a new feature to Arduino 1.5+. Full support for Transactional SPI should be available soon.
* I2C. Working, but is a software implementation, and has a maximum speed of around 250kbps. Speed improvements 
* EEPROM. Untested. An EEPROM library written for the Maple IDE is part of the repository, but has not been tested its likely it doesn't work.
* FreeRTOS. Untested. Sources from the original Maple IDE installer. (see also http://forums.leaflabs.com/topic.php?id=221)
* Liquid Crystal.Untested. Sources from the original Maple IDE installer. 
* Servo. Untested. Sources from the original Maple IDE installer. 
* RTClock. Untested. Sourced from GitHub ( see https://github.com/bubulindo/MapleRTC )

####Third party libraries

#####Tested and working
* I2CDev
* NRF24L01 reported to be working by @swe-dude on the Arduino forum
* LiquidCrystal_I2C - modified version of this library was supplied by @swe-dude has been added to the libraries folder.

#####Not working

* RadioHead 
* OneWire - loads of platform specific code in this library, controlled by #if's. STM32 specific code would need to be added. This library is currently hosted by PJRC (Teensy) so its hard to know whether Paul would be happy to include code for what is effectively competition to his Teensy product.  We may need to host this in the libraries folder.
