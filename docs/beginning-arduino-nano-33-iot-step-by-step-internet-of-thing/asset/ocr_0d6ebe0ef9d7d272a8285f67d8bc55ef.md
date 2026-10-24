# Analog I/O

Arduino Nano 33 IoT provides analog I/O to enable us to make interaction with sensor and actuator devices. We can see analog I/O pins with labeling Ax, where x is analog pin number. You can see these labels on the back of Arduino Nano 33 IoT.

You can see these pins in Figure 2-9. Arduino Nano 33 IoT has eight analog inputs (ADCs) and one analog input (DAC). For ADC model, Arduino Nano 33 IoT provides ADC resolution with 8-, 10-, and 12-bit. Furthermore, DAC model has a 10-bit resolution.

For our demo, we use the analog sensor TMP36. It’s a temperature sensor. You can also use TMP36 module like thermal module from Linksprite, [https://www.linksprite.com/wiki/index.php?title=Thermal_Module](https://www.linksprite.com/wiki/index.php?title=Thermal_Module). You can perform wiring as shown in Figure 2-11. You can build this following wiring:

+   - TMP36 module VCC is connected to Arduino 3.3V
+   - TMP36 module GND is connected to Arduino GND
+   - TMP36 module SIG is connected to Arduino analog A0

![](bbox=[0.169, 0.594, 0.871, 0.846])

**Figure 2-11.** *Wiring for analog sensor and Arduino Nano 33 IoT*