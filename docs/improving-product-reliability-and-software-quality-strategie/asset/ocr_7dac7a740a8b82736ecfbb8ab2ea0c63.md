```
// bool UartRead() reads data from the UART.
// returns TRUE if successful, FALSE if read fails.
// int dataLength: Size of read buffer.
// out String receiveBuffer: The data read from the UART.
// out int returnLength: The length of the returned data.
// out String errorMessage: Error message if call fails. Empty if successful.
//
bool UartRead(int dataLength, out String receiveBuffer, out returnLength, out String errorMessage);

// bool UartWrite() writes data to the UART.
// returns TRUE if successful, FALSE if write fails.
// int dataLength: Size of data to write.
// out String writeBuffer: The data to write to the UART.
// out String errorMessage: Error message if call fails. Empty if successful.
//
bool UartWrite(int dataLength, out String writeBuffer, out String errorMessage);
```

Figure 12.4 Sample UART HAL code.

hardware. The only code that has to change when the hardware implementation changes is the HAL code itself and generally the changes to this code are easy to test. In fact, creating and maintaining HALs also makes it more efficient to perform design verification (DV) on the hardware itself by allowing the DV code to be reused as well as the application code. Thus, the hardware team will also be able to more efficiently provide more stable hardware to the software team when the hardware and software integration tasks begin.

Generally, the hardware teams create and maintain the HALs. Since the hardware developers may be changing the register maps from project to project, or even during the course of developing a particular component, it is most efficient for them to also change the implementation of the HALs in conjunction with the hardware changes.

For maximum payback, HALs should be created for functions that are common to many hardware implementations. This would include generic common functionality such as buses and communications interfaces like USB, JTAG, UART, and SPI. For functionality common to your hardware types, HALs would also be highly beneficial as well. For example, developing HALs to control motors, perform pulse width modulation (PWM), control power supplies, and so on would probably all deliver good ROIs.

A sample HAL interface to read from or write to a UART is shown in Figure 12.4.

Here are three guidelines to follow to create good HALs:

+   1. HAL interfaces should be named clearly with explicit names so that they are easy to understand and for the higher-level software to consume. If HALs’ function names are cryptic, they can confuse the consumer of the HALs.
+   2. Most HALs should support parameters for initialization and or to pass values between the software and the hardware.
+   3. HALs should be kept at the highest level of abstraction of abstraction possible without incorporating any application code level logic.

Sometimes specific registers need to be read or written by higher-level code. Often, this is the case with DV code where a test is performed and then registers are read to confirm they are correct. A HAL can be provided to generically read or write a register. Even though code to read or write a register is easy to write, repetitively implementing such code is an opportunity to introduce defects.