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
* Adafruit_ILI9341 - Working and test by several people on different displays using hardware spi port 1.
* Adafruit_ILI9341_STM - Working, further modified to use DMA transfers, improving speed. Uses spi port 1
* ILI9341_due_STM - Based on DUE/Teensy ILI9341 library. Uses DMA and several other optimizations. Also supports multiple font types. Uses more flash and RAM than the standard ILI9341, but performs much better. Recommended for high refresh rates. Uses spi port 1. Should work on Arduino AVR, Due, Teensy, and Maple STM32 boards.
* Adafruit_GFX_AS - Based on Adafruit GFX_AS library, it adds support for more fonts. Working and tested in Maple mini and ILI9341 display.
* ILI9163 - Working but pending some updates to SPI library, so it is not added to the main repository yet. Works with and without DMA mode in SPI1. Available at the moment in fork https://github.com/victorpv/TFT_ILI9163C. This library requires that either you disable these 3 defines in the .h file: "#define SPI_16BIT" "#define SPI_MODE_DMA 1" "#define SPEED_UP 1" or you download the modified SPI.cpp and SPI.h from https://github.com/victorpv/Arduino_STM32/tree/master/STM32F1/libraries/SPI/src to replace the SPI files in your libraries folder, as they include several new functions. Performance is greatly improved by using DMA and/or 16bit transfers, but is still fairly good without those.

#####Not working

* RadioHead 
* RTClock. This library is in the untested sub folder of Libraries, as it currently doesn't compile. It was sourced from GitHub ( see https://github.com/bubulindo/MapleRTC ), however to work, the library needs changes to the core of libmaple, and the replacement core files are not compatible with the latest version of libmaple. 
* FreeRTOS. Untested. Sources from the original Maple IDE installer. (see also http://forums.leaflabs.com/topic.php?id=221)
* SdFat from https://github.com/victorpv/SdFat or https://github.com/greiman/SdFat. It is partially ported but have some bugs that make it unreliable. Anyone with knowledge can contribute to solve the bugs, or hold on a couple of weeks until they are corrected.
