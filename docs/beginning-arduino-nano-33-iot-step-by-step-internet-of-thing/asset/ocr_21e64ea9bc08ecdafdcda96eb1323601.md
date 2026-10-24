![](bbox=[0.131, 0.117, 0.909, 0.433])

*Figure 1-8.* Blinking LED on Arduino Nano 33 IoT

# How does it work?

Arduino Nano 33 IoT board has one built-in LED on digital pin 13. In our program, we set digital pin 13 as digital output using pinMode(). We initialize this data on the setup() function.

```
```cpp
void setup() {
  // initialize digital pin LED_BUILTIN as an output.
  pinMode(LED_BUILTIN, OUTPUT);
}
```
```

The Arduino program defines LED_BUILTIN for a general of built-in LED pin. We can set the pin as output mode by giving a value, OUTPUT. Now our program will run continuously on the loop() function. We turn on LED and then turn off the LED. We can use digitalWrite() to perform on/off on the LED. Set the value to HIGH to turn on the LED. Otherwise, we can turn off the LED by sending a value of LOW on the digitalWrite() function. We also set a delay for turning the LED on/off. We set 1000 ms on the delay() function.