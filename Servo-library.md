Notes on the Servo library

The Servo library uses hardware PWM, and is not available on all pins.

Also, some pins hardware PWM pins are shared with other functions e.g. Serial3, hence you may need to change the pin numbers in your code to use PWM and Serial3 etc

For details on which which pins support PWM see the board.cpp file for the variant you are using e.g. for Maple mini the list is

extern const uint8 boardPWMPins[BOARD_NR_PWM_PINS] __FLASH__ = {
    3, 4, 5, 8, 9, 10, 11, 15, 16, 25, 26, 27
};

Note to resolve these pin number back to read port and pin numbers on the STM32 device, please refer to the PIN_MAP table also in the board.cpp file.