This is an incomplete list of things you need to do to add a new board to the STM32F1 boards

1. Look at the existing boards and see which one most closely matches your board. The primary difference will be which processor you are using. e.g. STM32F103C or STM32F103Z series.
Look in the boards.txt file at the define for the MCU  e.g. -DMCU_STM32F103CB
2. For the board in boards.txt that closely matches your board, look at the  "variant" the board in question uses. e.g. maple_mini.build.variant=maple_mini  would indicate that the maple_mini board is a variant called maple_mini. This is the folder name of the variant
3. In the STM32F1 folder find the variants folder, and copy the variant folder for the existing board that most closely matches your new board e.g. STM32F1\variants\maple_mini -> STM32F1\variants\my-amazing-board
4. In boards.txt copy the block of data for the closest matching board, and rename it acordingly - the docs on boards.txt are available in Arduino's third party hardware specification (NEED TO ADD LINK HERE ;-) and change the name and variant and MCU etc etc to match your board, e.g. this will include flash and ram sizes and upload type
5. In your variant folder, the things you are most likely to need to change are the PIN_MAP array in board.cpp and also the enum that maps the STM GPIO names e.g. PA8, PB1 etc to the PIN_MAP array. This is a one to one mapping, ie the first enum value is the first element in the PIN_MAP array
6. You may also need to change pins_arduino.h
7. You may also need to change the linker script files in the ld. Note the linker script file is specified in the ldscript data in boards.txt e.g maple_mini.build.ldscript=ld/flash.ld  
The linker scripts probably don't need to be renamed, but they may need to be updated to match the processor memory size etc

8. restart the Arduino IDE, and try your new board. If it doesnt compile or doesnt upload... you have done something wrong... Turn on Verbose compile and upload in the preferences, and use the errors to determine what needs to be done to make it compile


