The SPI library generally operates in the same way as the AVR Arduino library in IDE version 1.0.

Transactional SPI first introduced in IDE 1.5.x has not been fully implemented yet, but some of the examples in the IDE that use SPI e.g. SD->CardInfo have been tested and work OK.

Speed of the SPI will be different from that of AVR Arduino's because the clock rate on the STM32's is not the same as the AVR, (its 72MHz instead of 16MHz)


Enhancements to my original conversion of the Maple "HardwareSPI" library have been made by @timschuerewegen, who's comments are shown below.


>I have made some improvements to the SPIClass class. The "setClockDivider" method needed weird and >limiting values (i.e. no way to set a 36 Mhz clock for SPI 1) and the "transfer" and "write" methods >were not waiting for the SPI busy flag to be cleared. I also added a "setFrequency" method.
>
>SPI.setClockDivider(SPI_CLOCK_DIV2) gives you the fastest clock, 36 Mhz (SPI 1) or 18 Mhz (SPI 2)
>...
>SPI.setClockDivider(SPI_CLOCK_DIV256) gives you the slowest clock, 281.250 Khz (SPI 1) or 140.625 Khz (SPI 2)
>
>SPI.setFrequency(SPI_36MHZ) gives you a 36 Mhz clock (only supported by SPI 1)
>SPI.setFrequency(SPI_18MHZ) gives you a 18 Mhz clock
>...
>SPI.setFrequency(SPI_281_250KHZ) gives you a 281.250 Khz clock
>SPI.setFrequency(SPI_140_625KHZ) gives you a 140.625 Khz clock (only supported by SPI 2)

