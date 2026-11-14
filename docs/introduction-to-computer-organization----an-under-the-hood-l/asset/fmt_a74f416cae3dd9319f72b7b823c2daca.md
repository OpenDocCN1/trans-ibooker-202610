
# Logic Gates

![](`image_fi/book_art/chapterart.png`)

In the previous chapter, you learned about Boolean algebra expressions and how to implement them using logic gates. In this chapter, you'll learn how to implement logic gates in hardware by using transistors, the solid-state electronic devices used to implement the on/off switches we've been discussing throughout this book.

To help you to understand how transistors operate, we'll start with a simple introduction to electronics. From there, you'll see how transistors can be connected in pairs to switch faster and use less electrical power. We'll end the chapter with some practical considerations regarding the use of transistors to construct logic gates.

## Crash Course in Electronics

You don't need to be an electrical engineer to understand how logic gates work, but some understanding of the basic concepts can help. This section provides a brief overview of the fundamental concepts of electronic circuits. We'll begin with two definitions.

Current refers to the movement of electrical charge. Electrical charge is measured in coulombs. A flow of one coulomb per second is defined as one ampere, often abbreviated as amp. Current flows through an electrical circuit only if there is a completely connected path from one side of the power source to the other side.

Voltage refers to the difference in electrical energy per unit charge, also called potential difference, between two points in an electrical circuit. One volt is defined as the electrical difference between two points on a conductor (the medium the current flows through) when one ampere of current flowing through the conductor dissipates one watt of power.

A computer is constructed from the following electronic components:

- Power source that provides the electrical power
- Passive components that affect current flow and voltage levels, but whose characteristics cannot be altered by another electronic component
- Active components that switch between various combinations of the power source, passive components, and other active components under the control of one or more other electronic components
- Conductors that connect the other components together

Let's look at how each of these electronic components works.

### Power Supplies and Batteries

In almost all countries, electrical power comes in the form of alternating current (AC). For AC, a plot of the magnitude of the voltage versus time shows a sinusoidal wave shape. Computer circuits use direct current (DC) power, which, unlike AC, does not vary over time. A power supply is used to convert AC power to DC, as shown in [Figure 5-1](#c05.xhtml#figure5-1).

![Figure 5-1: AC/DC power supply](`image_fi/500099c05/f05001.png`)

Figure 5-1: AC/DC power supply

Batteries also provide DC electrical power. When drawing circuits, we'll use the symbol for a battery ([Figure 5-2](#c05.xhtml#figure5-2)) to designate a DC power supply. The power supply in [Figure 5-2](#c05.xhtml#figure5-2) provides 5 volts DC.

![Figure 5-2: Circuit symbol for a 5-volt DC power source](`image_fi/500099c05/f05002.png`)

Figure 5-2: Circuit symbol for a 5-volt DC power source

Throughout this book, you've seen that everything that goes on in a computer is based on a system of `1`s and `0`s. But how are these `1`s and `0`s physically represented? Computer circuits distinguish between two different voltage levels to provide logical `0` and `1`. For example, logical `0` may be represented by 0 volts DC and logical `1` by 5 volts DC. The reverse could also be implemented: 5 volts as logical `0` and 0 volts as logical `1`. The only requirement is that the hardware design be consistent. Luckily, programmers don't need to worry about the actual voltages used—that's best left to the computer hardware engineers.

Because computer circuits are constantly switching between the two voltage levels, when the voltage is suddenly switched from one level to another, computer hardware engineers need to consider the time-dependent characteristics of the circuit elements. We'll look at these characteristics in the following section.

### Passive Components

All electrical circuits have resistance, capacitance, and inductance. These electromagnetic properties are distributed throughout any electronic circuit:

1. Resistance Impedes current flow, thus dissipating energy. The electrical energy is transformed into heat.

2. Capacitance Stores energy in an electric field. Voltage across a capacitance cannot change instantaneously.

3. Inductance Stores energy in a magnetic field. Current through an inductance cannot change instantaneously.

It takes time for energy to be stored as an electric field in capacitance, or as a magnetic field in inductance, so these two properties impede changes in voltage and current. These two properties are lumped together with resistance and called impedance. The impedance to changes slows down the switching that takes place in a computer, and the resistance consumes electrical power. We'll be looking at the general timing characteristics of these properties in the remaining part of this section but will leave a discussion of power consumption to more advanced books on the topic.

To get a feel for the effects of each of these properties, we'll consider the discrete electronic devices that are used to place these properties in a specific location in a circuit: resistors, capacitors, and inductors. They are part of a broader class of electronic components called passive components, which cannot be controlled electronically. They simply consume or store the energy.

[Figure 5-3](#c05.xhtml#figure5-3) shows the circuit symbols for the passive electronic devices we'll be discussing. Each is described next.

![Figure 5-3: Circuit symbols for passive devices](`image_fi/500099c05/f05003.png`)

Figure 5-3: Circuit symbols for passive devices

#### Switches

A switch can be in one of two positions—open or closed. In the open position, there is no connection between the two ends. When closed, the connection between the two ends is complete, thus conducting electricity. The symbol in [Figure 5-3](#c05.xhtml#figure5-3) (d) typically indicates a switch that is activated manually. In "Transistors" on page 100, you'll learn that a computer uses transistors for open/closed switches, which are controlled electronically, thus implementing the on/off logic that forms the basis of a computer.

#### Resistors

A resistor is used to limit the amount of current in a specific location in a circuit. By limiting the current flow into a capacitor or inductor, a resistor affects the time it takes for these other devices to build up their energy storage. The amount of resistance is usually chosen in conjunction with the amount of capacitance or inductance to provide specific timing characteristics. Resistors are also used to limit current flowing through a device to nondestructive levels.

As it limits current flow, a resistor irreversibly transforms the electrical energy into heat. A resistor doesn't store energy, unlike a capacitor or inductor, which can return the stored energy to the circuit at a later time.

The relationship between voltage and current for a single resistor is given by Ohm's law,

```
V(t) = I(t) × R
```

where `V(t)` is the voltage difference across the resistor at time `t`, `I(t)` is the current flowing through it at time `t`, and `R` is the value of the resistor. Resistor values are specified in ohms.
