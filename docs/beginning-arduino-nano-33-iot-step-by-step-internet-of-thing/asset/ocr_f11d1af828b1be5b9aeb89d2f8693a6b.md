# How does it work?

First, we initialize our I2C, Serial, and PCF8591 AD/DA converter module. We define I2C address channel. This is done on the setup() function.

```
```cpp
#include "Wire.h"
#define PCF8591 0x48 // I2C bus address
#define PCF8591_ADC_CH0 0x00 // thermistor
#define PCF8591_ADC_CH1 0x01 // photo-voltaic cell
#define PCF8591_ADC_CH2 0x02
#define PCF8591_ADC_CH3 0x03 // potentiometer
byte ADC1, ADC2, ADC3;

void setup()
{
  Wire.begin();
  Serial.begin(9600);
}
```
```

We can read sensor data on the loop() function. To read thermistor data, we open I2C using Wire.beginTransmission() with passing PCF8591. Then, select a channel for thermistor with value PCF8591_ADC_CH0 using Wire.write(). We close transmission by calling Wire.endTransmission(). We read sensor data with 2 bytes using the Wire.requestFrom() function.

```
```cpp
void loop()
{
  // read thermistor
  Wire.beginTransmission(PCF8591);
  Wire.write((byte)PCF8591_ADC_CH0);
  Wire.endTransmission();
  delay(100);
```
```