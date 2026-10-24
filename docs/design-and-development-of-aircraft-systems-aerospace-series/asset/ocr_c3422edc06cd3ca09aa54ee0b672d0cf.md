![](bbox=[0.11, 0.12, 0.86, 0.48])

Figure 5.23 Overall FCS architecture prior to design (Weller 2018).

# 5.7.3 Example 3: Radar System

This example has been chosen as it appears to be a relatively simple, although very high value, system and it is used as a verification example in Chapter 7. The customer’s operational requirement will include:

+   - detecting a particular set of targets
+   - detecting targets at a particular range
+   - discriminating target from background clutter
+   - detecting, classifying, identifying, tracking, and locking on to targets.

Figure 5.24 shows a sketch of a system that will meet these requirements. This is the beginning of an integrated weapon system in which the radar is physically integrated into the airframe and is then integrated with the aircraft avionics, the armaments system, and the cockpit to provide a system that can detect targets, identify, them, track them, and then engage them with missiles with a single pilot to perform these tasks and to fly the aircraft.

Figure 5.25 shows the main elements of the sketch put into an architecture to show the relevant interfaces. This architecture is an interconnection of the basic navigation systems, the radar sensor, and the armaments system – in a military aircraft. Thus there is no need for interconnection of commercial and military standards – all the sensors and data buses are designed to military standards. The resulting system is a comprehensive target acquisition and engagement system in a fast jet military aircraft operated by a single pilot.