Currently the libraries that come with the IDE work, (with some caveats), However very few third party libraries have been tested, and its likely that many third party Libraries don't currently work (or even compile).

####Built-in and Maple specific libraries

* SPI. Working. But lacks full Transaction support. Transactional SPI is a new feature to Arduino 1.6.x. Full support for Transactional SPI will be investigated when the functionality has matured and stabilized in the API.
* I2C. Working, However this is a software (bit-banged) implementation, and has a maximum speed of around 250kbps. Speed improvements. Hardware I2C has been tested by some users, but is not integrated into the normal Wire library.
* EEPROM. Tested, but data may not survive a reload of the sketch, as both the sketch and EEPROM data both reside in flash, and it will depend on the uploader, whether the data "page" is erased or not. (This aspect has jet to be fully tested)


#####Tested and working
* Liquid Crystal I2C - tested and working
* Liquid Crystal - I can't guarantee this works as this has not been tested.
* I2CDev - Tested by me (Roger Clark)
* NRF24L01 reported to be working by @swe-dude on the Arduino forum
* LiquidCrystal_I2C - modified version of this library was supplied by @swe-dude has been added to the libraries folder.
* OneWire - Updated by me (Roger Clark). Currently this uses direct hardware access via a small change to the way the pin macros work. However I have also submitted a pull request to Paul (PJCR) who manages the master repo for this library, to add a form of "fallback" support for architectures not specifically catered for in the code - but using normal API calls for digitalRead,digitalWrite and pinMode (rather than direct hardware access) as the 72MHz STM32F1 is plenty fast enought that direct hardware access is not required for the bus timings to work.
* Servo  (Tested and reported to be working by @ahull on the Arduino forum)

#####Not working

* RadioHead 
* RTClock. This library is in the untested sub folder of Libraries, as it currently doesn't compile. It was sourced from GitHub ( see https://github.com/bubulindo/MapleRTC ), however to work, the library needs changes to the core of libmaple, and the replacement core files are not compatible with the latest version of libmaple. 
* FreeRTOS. Untested. Sources from the original Maple IDE installer. (see also http://forums.leaflabs.com/topic.php?id=221)

