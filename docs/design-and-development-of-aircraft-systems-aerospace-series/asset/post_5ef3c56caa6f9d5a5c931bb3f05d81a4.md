
## 10.8 Integration of Avionics Systems

The previous discussion has related to some of the aircraft systems, but on a modern aircraft the avionics system is equally important. The avionics system is – as has previously been described – essentially the brains of the aircraft, helping the flight crew negotiate the busy airspace of today with precision and in safety. The key elements of the avionics system are shown in Figure 10.19. There are four main functional components:

- navigation: flight management, inertial navigation, satellite navigation, navigation aids, and terrain avoidance warning system (TAWS)
- flight control: air data, attitude systems, FBW, autopilot, and auto-throttle
- communications: high frequency (HF), approach aids, very high frequency (VHF), mode S and traffic collision avoidance system (TCAS), and satellite communications (SATCOM)
- displays: primary displays including an electronic flight instrument system (EFIS) providing flight information and an engine indicating and crew alerting system/electronic centralised engine monitor (EICAS/ECAM) providing system synoptic and status displays, aircraft system overhead panels, multi-purpose control and display units (MPDCUs), equipment control panels, and standby instruments.

These functions have significant interaction between them and with the aircraft systems already described.

![](img/68b1a140c4426ceff0475b2d902d98df_307_0.png)

Figure 10.19 Major avionics system functions.

Some of these systems and their inter-reactions can be neatly described as a series of nested control loops. Other inter-reactions are more subtle. Figure 10.20 shows the interaction between FBW, the autopilot and the flight management system (FMS) as a simplified series of nested control loops.

These control loops, starting from the simplest on the inside and working outwards towards the most complex, are as follows:

- **Primary flight control – ATA Chapter 27:** This is used to control the *aircraft attitude*. Inputs from the pilot’s controls are fed through the FBW system control computers to the flight control actuators that modify the aircraft attitude in response to the pilot’s commands. The aircraft dynamics and attitude sensors result in feedback to the pilot either visually or by means of the EFIS displays.
- **Autopilot – ATA Chapter 22:** Once the autopilot is engaged the aircraft is controlled by the next control loop, which controls the *aircraft trajectory*. By means of selecting speeds, height, heading and speed datums, and navigation and approach aids the pilot is able to accurately manage the aircraft trajectory during flight with much reduced work load. Control of the autopilot is achieved by the use of a flight control unit (FCU) or mode control panel situated on the flight deck glare-shield.
- **FMS – ATA Chapter 34:** The FMS effectively assists the flight crew in achieving the *aircraft mission*. For a civil airliner this involves executing departure and arrival procedures, and negotiating the navigation of the aircraft on a series of way points as the aircraft flies from the departure airport to the destination airport. The primary flight crew interface with the FMS is the multi-purpose control and display unit (MCDU) of which there are three units on the flight deck.

![](img/68b1a140c4426ceff0475b2d902d98df_308_0.png)

Figure 10.20 Inter-relationship of FBW, autopilot and FMS. MCDU, multi-function control and display unit; FCU, flight control unit; FMS, flight management system; AFDS, auto-pilot and flight director system; FBW, fly by wire.

It will be recognised that the co-ordination of the overall specification of all of these systems, including a range of equipment produced in different timescales with different technology baselines, is a mammoth task. The industry approaches this problem by using a series of equipment and technology specifications derived for and by the industry to be used worldwide. These specifications are controlled by the Air Radio Inc. (ARINC) organisation in the USA and the hierarchy of specifications as they presently exist at the time of writing is shown in Figure 10.21.

There are four series of ARINC specifications:

- **ARINC 400 Series:** These are termed the design foundation and relate to the earliest avionics specifications. Typical amongst this series are ARINC 429, which was used to specify the first civil serial avionics data bus, and ARINC 404A, which was an early packaging standard. Presently 21 specifications in the ARINC 400 series are still in use.
- **ARINC 500 Series:** As electronics became increasingly used in aircraft equipment a series of specifications was derived to deal with analogue equipment used in the older analogue – now called ‘classic’ – aircraft such as the DC-9, MD-10, A300, and early Boeing 737 and Boeing 747 aircraft. Typical examples include ARINC 578 and 579, which were used to define the characteristics of instrument landing system (ILS) and VHF omni-range (VOR) equipment, respectively Some 21 specifications in this series still exist.
- **ARINC 600 Series:** When the scale of the application of digital electronics to civil aircraft became clear this series became the vehicle for specifying enabling digital technologies. Examples are ARINC 629, which defines a 2Mb/s digital data bus used on the Boeing

![](img/68b1a140c4426ceff0475b2d902d98df_309_0.png)

Figure 10.21 Hierarchy of ARINC specifications.

777, and ARINC 600, which specifies advanced packaging techniques beyond the earlier ARINC 404A standard. Around 70 ARINC 600 series are presently in use.

- **ARINC 700 Series:** To define the new digital equipment used in the present modern generation of digital aircraft the ARINC 700 series evolved. Typical examples include ARINC 708 for a digital weather radar and ARINC 755 for a multi-mode receiver (MMR) incorporating GPS, ILS, and other RF receivers in a single unit. Some 66 ARINC 700 series specifications are presently in use and doubtless others are in evolving in draft form for the specification of new and emerging equipment. Details on these specifications made be found on the ARINC website.
- **ARINC 800 Series:** Specifications and reports define enabling technologies supporting the networked aircraft environment. Amongst the topics covered in this series is fibre optics used in high-speed data buses, A802-1 for fibre optic cables, and A808-1 for cabin distribution systems.
- **ARINC 900 Series:** ARINC characteristics define avionic systems in an integrated modular and/or networked structure. They include detailed functional and interface definitions.

## References

Airworthiness Circular AC 120-42A (2008) Extended Range Operation with Two-Engine Airplanes, 30 December 1988.

FAA/JASC(2002). FAA Joint Aircraft System/Component code tables and definitions. Issued February 2002. Google JASC Codes PDF. Accessed March 2019.

Moir, I. and Seabridge, A. (2008). Aircraft Systems, 3e. Wiley.

Moir, I. and Seabridge, A. (2013). Civil Avionics Systems, 2e. Wiley.

RTCA DO-178B, (1992) Software Considerations in Airborne Systems and Equipment Certification.

RTCA DO-254, (2000) Design Assurance Guidelines for Airborne Electronic Hardware.

SAE ARP 4754, Certification Considerations for Highly-Integrated or Complex Aircraft Systems, Society of Automobile Engineers Inc.

SAE ARP 4761, Guidelines and Methods for Conducting the Safety Assessment Process on Civil Airborne Systems, Society of Automobile Engineers Inc.

## 11

## Integration and Complexity

The Potential Impact on Flight Safety

### 11.1 Introduction

So far in this book, as in other aerospace series books by Moir and Seabridge, the broad assumption has been made that aircraft systems will be designed as individual entities. This is very often what happens in large organisations, generally as a result of their departmental or organisational structures, which are often formed around individual system responsibilities. The subject of ‘interconnectedness’ or interactions between systems has been introduced as a warning to designers to be aware of unintended consequences arising from the performance of their systems and the interactions that occur, directly or indirectly, with other systems in the total aircraft design.

Chapter 6 introduced the topic of systems integration as a normal part of the continuing development of the systems process; Chapter 5 introduced the topic of integrated systems architectures and a number of different systems examples were given. The content of those chapters is still relevant. In this chapter the results of the integration of physical systems, functional aspects and functions associated with automation, and potentially autonomous unmanned operations is examined. The purpose of the chapter is to discuss whether or not such continued integration of functions is likely to pose a threat to the continuing safe operation of the aircraft. The reason for examining this is not to impose a limitation on such innovations but to cause the managers of system integration – chief engineers, system integrators, systems managers, and systems engineers – to reflect on the process and to act to ensure that no such threat is posed.

What follows is a number of observations and examples, some based on experience, some based on published material, together with some warnings intended as guidance for systems engineers and integrators.

### 11.2 Integration

Integration can be characterised by the performance of many functions by a small number of units. It is more commonly applied to avionics-type systems, but is also found in major structural and mechanical systems such as aircraft powerplant and propulsion systems and even the airframe itself. In an integrated system the individual systems interact with each
