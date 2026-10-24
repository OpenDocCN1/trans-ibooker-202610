
![](img/68b1a140c4426ceff0475b2d902d98df_155_0.png)

![](img/68b1a140c4426ceff0475b2d902d98df_155_1.png)

**Figure 5.18** Illustration of integrated modular architecture from two modern aircraft.

- offers benefits of scale across the entire aircraft
- improves logistics for the airlines
- provides a scaleable architecture with scope for application to future projects.

The Boeing 787 uses a different approach with remote data concentrator units (equivalent to remote input/output [RIO]) to condition inputs and outputs, and general purpose modules to perform the processing task.

## 5.7 Example Architectures

This section will make use of the information presented previously in order to show how elements of different system architectures can be used to build architectures for individual and integrated systems. Some examples are given for different systems in the aircraft to illustrate what an architecture represents and how it is used in the design of a system. The architectures shown do not represent any specific aircraft, they are solely for example. No specific formal method is used for their representation and no method is recommended. The systems engineer must decide what is most suitable for their task, but must also ascertain if there is a common tool or method demanded by the project, in which case that must be adopted to ease communication.

### 5.7.1 Example 1: System Architecture

This architecture was shown previously in Figure 5.4 and is shown again here in Figure 5.19 simply to illustrate where the following examples fit.

![](img/68b1a140c4426ceff0475b2d902d98df_156_0.png)

Figure 5.19 A top-level system architecture.

![](img/68b1a140c4426ceff0475b2d902d98df_157_0.png)

**Figure 5.20** Example of a large military aircraft architecture.

The architecture can be developed further to show more detailed definition and to show how it can be used to further define requirements, responsibilities, and functional segregation. Figure 5.20 shows how the architecture can be used to segregate sub-system architectures in a military aircraft. For reasons of integrity it is necessary to separate the main functions of avionics, aircraft systems, the mission system, and the weapon system, as well as segregating their wiring harnesses. The main reasons are to ensure a suitable integrity for the aircraft systems functions and to apply specific armaments standards to the weapon system.

Usually this leads to use of military standards for the data bus, for example MIL-STD-1553B or STANAG 3910 for all functions. However, in the case of converting an existing commercial platform into a military type, for example for surveillance purposes, there may be sound reasons for keeping the commercial avionics and blending that with military standard systems. Figure 5.21 shows an architecture used as the basis for achieving such a system by using the mission computer as a buffer between the two systems.

### 5.7.2 Example 2: Flight Control System

In Figure 5.19 the flight control system (FCS) is shown as a single box as part of the airframe systems. Figure 5.22 shows the contents of this box as an FCS top-level generic architecture and shows some preliminary interfaces to the aircraft in the form of the displays and controls, the air data system, and the control surfaces.

This has been developed further to the architecture shown in Figure 5.23. This is a historic diagram since it shows a flight control structure developed for an experiment to demonstrate fly-by-wire control in an unstable aircraft, the fly-by-wire (FBW) Jaguar

![](img/68b1a140c4426ceff0475b2d902d98df_158_0.png)

**Figure 5.21** Allocating responsibilities between civil and military systems. DF, direction finding; ESM, electronic support measures; Nav Aids, navigation aids; Nav/FMS, navigation/flight management system; MAD, magnetic anomaly detector; TACAN, tactical air navigation.

![](img/68b1a140c4426ceff0475b2d902d98df_158_1.png)

**Figure 5.22** A generic FCS architecture.

(Weller 2018). It records some important decisions made on a quadruplex implementation of a software intensive system designed for high integrity. This architecture led on to a complete design that was successfully tested on a Jaguar aircraft and formed the basis for following aircraft in the UK such as EAP and Typhoon.

![](img/68b1a140c4426ceff0475b2d902d98df_159_0.png)

Figure 5.23 Overall FCS architecture prior to design (Weller 2018).

### 5.7.3 Example 3: Radar System

This example has been chosen as it appears to be a relatively simple, although very high value, system and it is used as a verification example in Chapter 7. The customer’s operational requirement will include:

- detecting a particular set of targets
- detecting targets at a particular range
- discriminating target from background clutter
- detecting, classifying, identifying, tracking, and locking on to targets.

Figure 5.24 shows a sketch of a system that will meet these requirements. This is the beginning of an integrated weapon system in which the radar is physically integrated into the airframe and is then integrated with the aircraft avionics, the armaments system, and the cockpit to provide a system that can detect targets, identify, them, track them, and then engage them with missiles with a single pilot to perform these tasks and to fly the aircraft.

Figure 5.25 shows the main elements of the sketch put into an architecture to show the relevant interfaces. This architecture is an interconnection of the basic navigation systems, the radar sensor, and the armaments system – in a military aircraft. Thus there is no need for interconnection of commercial and military standards – all the sensors and data buses are designed to military standards. The resulting system is a comprehensive target acquisition and engagement system in a fast jet military aircraft operated by a single pilot.

![](img/68b1a140c4426ceff0475b2d902d98df_160_0.png)

Figure 5.24 Scheme of a radar system.

![](img/68b1a140c4426ceff0475b2d902d98df_160_1.png)

Figure 5.25 Architecture of a radar system.

### 5.7.4 Example 4: Vehicle Systems Management

This rather lengthy example explains how a computing system for the set of power and mechanical systems, known as vehicle systems, was developed for a range of systems that are described in section 12.2. This is a historical example but it illustrates how a whole

system can be developed using different types of architectural examples to arrive at a robust solution. The final system was demonstrated on an experimental aircraft and it formed the basis for systems in use today, albeit in rather more modern implementations. The following terminology has been used:

- vehicle systems – the individual power and mechanical systems of the aircraft
- VMS – the system that controls all vehicle systems
- VMS architecture – the VMS processors and data bus as part of VMS
- VMS processor – the processing units associated with VMS
- processor/memory – the computing system installed in each VMS processor
- I/O – signals associated with the vehicle systems.

The process began with the desire to develop a computing system for the interfacing, control, and management of a number of systems. The nature of the problem is illustrated in Figure 5.26.

These systems were a challenge because they were predominantly mechanical or electrical and usually involved a transfer of significant amounts of energy to perform a function. They were also directly associated with the safe operation of the aircraft. This is in contrast to the avionics systems, which are predominantly managing information. They had a diverse range of input and output characteristics with a wide range of sensor types.

There were some decisions that could be made early in the project based on the integrity of the systems that would determine which system could be entrusted to computer control and which were of a level of integrity that ruled them out of this system. This was decided on the basis of the need for complex functions to be performed balanced against a need for independence for safety reasons, and the result is illustrated in Figure 5.27.

Discussion with the engineers of individual systems led to an agreement on which systems were suitable and which were not, based on the integrity of each system, the

![](img/68b1a140c4426ceff0475b2d902d98df_161_0.png)

Figure 5.26 Developing an integrated system for the vehicle systems – the start.
