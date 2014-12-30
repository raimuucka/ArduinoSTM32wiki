Support has been added for Mircroduino boards, thanks to Ian Hawkin.

See 

https://www.microduino.cc/module/view?id=53e9d9563f6ada00004fb0c3


Note. SPI currently doesn't compile for Microduino as the NSS pin from the STM32 is not connected to physical pin on the edge of the board. 
However as NSS is not currently used by the SPI code, we will implement a workaround / hack shortly.

