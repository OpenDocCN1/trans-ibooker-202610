# # Review Arduino Nano 33 IoT Board

Arduino Nano 33 IoT is built from ARM Cortex M0 32-bit SAMD21. The board also has a radio module, NINA-W102, from u-blox. This module is designed for data communication over WiFi and Bluetooth. You can read a detailed specification of Arduino Nano 33 IoT on Table 1-1.

Since Arduino Nano 33 IoT has some digital and analog I/O, we extend the board capabilities by wiring with other sensors or actuators. We also use universal asynchronous receiver/transmitter (UART), serial peripheral interface (SPI), and interintegrated circuit (I2C) protocols to communicate with other devices.

**Table 1-1.** *A Specification of Arduino Nano 33 IoT*

| Features | Notes |
|---|---|
| Microcontroller | SAMD21 Cortex-M0+ 32-bit |
| Radio module | u-blox NINA-W102 |
| Secure module | ATECC608A |
| Operating voltage | 3.3V |
| Input voltage | 21V |
| DC current per I/O pin (limit) | 7 mA |
| Clock speed | 48 Mhz |
| CPU flash memory | 256 KB |
| SRAM | 32 KB |
| EEPROM | None |
| Digital I/O | 14 |
| PWM pins | 11 (2, 3, 5, 6, 9, 10, 11, 12, 16 / A2, 17 / A3, 19 / A5) |