# Introduction

We can say Arduino is a platform since Arduino as a company provides hardware and software. To build programs for Arduino Nano 33 IoT boards, we can use Arduino Sketch. This program uses C/C++ dialects as its language style.

In this chapter, we learn how to build programs for Arduino Nano 33 IoT. This is one of various Arduino models. The Arduino Nano 33 IoT board uses WiFi and Bluetooth modules to connect to a network. WiFi is a common network that people use to access Internet. Bluetooth is a part of wireless personal network (WPAN) that enables communication with other device in short distance.

We use Arduino software to build Arduino programs. This tool uses the Sketch program that uses C++ dialects. In the next section, we start to learn Sketch programming.

# Basic Sketch Programming

In this section, we learn about Sketch programming language. Technically, Sketch uses C++ dialects, so if you have experience with C++ programming language, you can skip this section.

# Main Program

The Arduino program has a main program to perform tasks continuously. When we create a program using Arduino software, we have skeleton codes with two functions: setup() and loop(). You can see the complete codes as follows.

```
void setup() {
  // put your setup code here, to run once:
}
```