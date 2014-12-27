## Windows

A number of different methods to upload are available to Windows users. The choice of method is mainly dependent, on the type of board this is being used.  

### Maple and Maple mini

The primary method to upload on the Maple and Maple mini is via the built in USB connection, via the LeafLabs drivers. (Please see details and issues with these drivers on the Maple / Maple mini page).

### Generic / unbranded STM32F103 boards

The easiest way to upload to these board is to use a USB to Serial adapter connected to Hardware Serial 1  (Pins PA9 and PA10).

Additionally to enter serial bootloader mode, these boards, but be configured so that Boot0 is HIGH and Boot1 is LOW. On most boards these pins are either connected to jump links or to switches.
With Boot0 HIGH and Boot1 LOW, after reset the board enters serial bootloader mode; which is a capability built into the STM32 microprocessor (its not a piece of code that resides in any piece of internal memory that can be read or written to or updated).

### Boards with on-board STLink capability 

STM Nucleo boards come with an onboard STLink, for these boards, the STLink protocol can be used, but currently only upload is supported, not Serial comms via STLink. Hence this is not generally the preferred option even for boards with this capability 