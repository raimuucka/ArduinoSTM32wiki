The original Maple IDE used some non-standard API calls, including SerialUSB and also the requirement to instantiate HardwareSPI in the sketch.

SerialUSB on Maple builds has now been changed to Serial.  Serial1 is Hardware serial 1, Serial2 is Hardware serial 2 etc (all 4 hardware serial channels are available)

To use SPI, just include <SPI.h> and use the global instance SPI which is created in the SPI class. i.e This is the normal Arduino way to use SPI.

AnalogWrite. This has been modified so that you no longer need to set the pinMode to PWM as this is done inside the analogWrite function. 
Scaling of the input value has also been changed to conform with the Arduino API, i.e values need to be 0 to 255 instead of 0 to 65535.

As the call to pinMode takes a small amount of time to complete. pwnWrite() can be used, as this doesn't set the pinMode and hence will be slightly faster.

Note. There may be other differences which I have omitted. Please contact me if you find an omission.