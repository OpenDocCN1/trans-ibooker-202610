![](bbox=[0.131, 0.117, 0.911, 0.516])

**Figure 2-19.** Connecting MISO and MISO pins from Arduino SPI

To access the SPI interface on Arduino Nano 33 IoT, we can use the SPI library. You can obtain a detailed information about this library at this link, [https://www.arduino.cc/en/Reference/SPI](https://www.arduino.cc/en/Reference/SPI).

Now we can build the Arduino program. Our program will send data to SPI and receive data from SPI. Open Arduino software and then write this complete program.

```
```cpp
#include <SPI.h>

byte sendData,recvData;
void setup() {
  SPI.begin();
```
```