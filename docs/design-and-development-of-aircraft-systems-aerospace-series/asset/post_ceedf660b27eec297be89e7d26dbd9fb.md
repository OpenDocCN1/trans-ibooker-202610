
![](img/68b1a140c4426ceff0475b2d902d98df_47_0.png)

Figure 2.2 A hierarchy of systems requirements.

![](img/68b1a140c4426ceff0475b2d902d98df_47_1.png)

Figure 2.3 An extended view of the systems requirements hierarchy.

This flow down of requirements can be extended further to show that they need not necessarily emerge from the customer’s requirement, as illustrated in Figure 2.3.

There is a key input of regulatory requirements from standards and directly from regulatory bodies. These regulatory inputs may be determined by flight safety, health and safety

![](img/68b1a140c4426ceff0475b2d902d98df_48_0.png)

Figure 2.4 A product view of systems.

or by the need to meet environmental legislation. These are requirements often formed by international bodies and made mandatory through aviation regulatory bodies. A most important aspect of the figure, which has a strong influence on the design of systems, is shown by the derived requirements. These emerge as individual systems develop and relationships between systems begin to emerge as interfaces, sharing of functions, and physical interactions. Great skill is required by engineers to identify, define, and record these requirements. This is where the majority of design takes place in aerospace organisations and the result is the design data set for the product.

The resulting product view is shown in Figure 2.4, in which an overall system architecture consists of a number of individual sub-systems and sub-sub-systems complemented by components. The architectural principles described in the uppermost system level must be obeyed.

## 2.4 Aircraft Systems of Interest

The Air Transport Association (ATA) chapter numbering system has been used for many years to provide a common referencing system for all civil aircraft. The system is controlled and published by the ATA, an airline association based in the USA for over 75 years that harmonises the requirements of the US air transport system. This organisation has recently been renamed Airlines for America (A4A), although its Charter remains much the same. The key pillars of the organisation are safety, engineering and maintenance, and flight operations and air traffic management.

The ATA chaptering system provides a unified referencing system whereby aircraft systems share common identifiers irrespective of aircraft type, e.g. Chapter 24 represents the aircraft electrical system whether the aircraft is a B747 or a small business jet. For the air transport engineering community this referencing system provides a consistent framework for aircraft technical documents and maintenance manuals.

A simplified version of the ATA referencing system is portrayed in Figure 2.5. Classic ‘avionics’ systems such as auto-flight, communications, recording and indicating (displays), and navigation are shown in segment 9.2. The highly integrated nature of modern air transport aircraft means that some or many ATA chapter functions may inter-react to provide a top-level aircraft function. An example of the levels of integration necessary to provide the mission management function of a typical aircraft operating in our skies today is given Chapter 10, Section 10.6.

The ATA 100 system was used until the early 2000s but has now been superseded by Joint Aircraft System/Component (JASC) four-digit codes (FAA 2002). This system provides a similar role to ATA but is now universally used for modern aircraft. For users of legacy aircraft and documentation systems the ATA referencing system has been retained in this edition.

Many of the systems described above are, in fact, collections of sub-systems, which, in combination, perform as a single system. Each individual sub-system will be designed and mechanised in different ways to perform its function, although there may be some over-riding rules for design laid down by the main system organisation.

The modern aircraft is also a system. The modern military aircraft is a collection of inter-dependent sub-systems designed for a specific role. The modern civil aircraft is similarly a

![](img/68b1a140c4426ceff0475b2d902d98df_49_0.png)

Figure 2.5 Simplified representation of the ATA referencing system. Now superseded by FAA JASC Codes (FAA 2002).

![](img/68b1a140c4426ceff0475b2d902d98df_50_0.png)

collection of sub-systems, many of them identical in their principles of operation to the military aircraft, although there are some that are significantly different. These sub-systems are designed to perform a specific individual task and are combined to form the whole aircraft in which the combination of the individual systems clearly performs a role that is greater than the sum of the individual parts. In other words, the sub-systems are acting in a synergistic manner.

The sub-systems can all be considered generically, and the remainder of this book will attempt to do that, whilst identifying any differences between the two types of aircraft.

Figure 2.6 shows the aircraft as a set of sub-systems that are common to most commercial and military types. These sub-systems map onto the domains in which many engineers are educated or into which they develop in their careers. Many prime contractor or aircraft manufacturer organisations are structured in this way.

The sub-systems show some interesting integration characteristics which must be taken into account in the overall system design. The vehicle systems show some very strong physical interactions with the airframe or structure. This arises because systems such as propulsion and fuel are very much a part of the structure. In a commercial aircraft the engines are usually incorporated in pods suspended from the wings and the thrust loads must be accounted for in the design of the wing; in a military fast jet the engines with their intakes and jet pipes must be incorporated into the structure. The fuel system tanks are similarly incorporated into the structure, especially the wing tanks. Heat and loads generated

![](img/68b1a140c4426ceff0475b2d902d98df_51_0.png)

## Figure 2.7 An example of system interactions (Moir and Seabridge 2008).

in many of the vehicle systems are translated into the structure. An example of such interaction is illustrated in Figure 2.7 and an explanation follows.

This example of significant interaction between systems shows how various systems operate together to reject waste heat from the aircraft. Heat is generated when fluids are compressed and also by energy conversion processes that are not totally efficient. Figure 2.7 depicts the interaction of several major systems, this time within the context of a civil aircraft. The diagram illustrates how a total of eight heat exchangers across a range of systems use the aircraft fuel and ambient ram air as heat sinks into which waste heat may be dumped. Starting with the engine the process is as follows:

- 1) Air extracted from the engine fan casing is used to cool bleed air tapped off the intermediate or high-pressure compressor (depending on engine type).
- 2) Air is used to cool engine oil in a primary oil cooler heat exchanger.
- 3) Fuel is used to cool engine oil in a secondary oil cooler heat exchanger.
- 4) The electrical integrated drive generator (IDG) oil is cooled by air.
- 5) The hydraulic return line fluid is cooled by fuel before being returned to the reservoir.
- 6) Aircraft fuel is cooled by an air/fuel heat exchanger.
- 7) Ram air is used in primary heat exchangers within the air-conditioning pack to cool entry bleed air prior to entering the secondary heat exchangers.
- 8) Secondary heat exchangers further cool the air down to temperatures suitable for mixing with warm air prior to delivery to the cabin.

The avionics and mission systems are mainly based on information structures and although there are demands for installation and low drag, much of the integration is based on data bus networks.

These systems and sub-systems can be further broken down into individual sub-sub-systems as described below.

### 2.4.1 Airframe Systems

The airframe can be viewed as a system since it is a complex and integrated set of structural components that supports the mass of systems and passengers, and carries loads and stresses throughout the structure. The airframe is designed and constructed as a set of sub-systems that are integrated to form the whole structure. This book will not describe the airframe any further, but will concentrate on the remainder of the aircraft systems, those that provide the airframe with the capability to perform its role.

### 2.4.2 Vehicle Systems

Aircraft vehicle systems are also known as general systems or utility systems. Many of these systems are common to both civil and military aircraft; they are a mixture of systems with very different characteristics. Some are high speed, closed loop, high integrity control such as flight controls; others are real-time data gathering and processing with some process control functions such as the fuel system. Yet others are simple logical processing such as undercarriage sequencing.

What they have in common is that they all affect flight safety in some way – in other words a failure to operate correctly may seriously hazard the aircraft, crew or passengers.

The functions of many of these systems are performed by software-based control units, either individual units or an integrated processing system such as a vehicle systems management system. This means that the software must be designed to appropriate levels of robustness. The following list provides a summary of the main purpose of each vehicle system.

- *Propulsion system*: provides the primary source of thrust and motive power via pilot demands, and electronic and hydro-mechanical fuel controls. This system provides the thrust and energy for flight, and also the motive power for the generation of electrical, hydraulic, and pneumatic systems.
- *Fuel system*: provides a source of energy for the propulsion system, which consists of tanks, a quantity measuring system, pumps, valves, non-return valves, and pipes to transfer fuel from tank to tank and to the engines. The fuel system is also used for the centre of gravity control and is the recipient of thermal energy from other systems as a result of its use in heat exchangers.
- *Electrical power generation and distribution system*: generates AC and DC power from the engine-connected generators and batteries, and distributes the power to all connected equipment, whilst protecting the electrical bus-bars and the electrical wiring harnesses from connected faults.
- *Hydraulic power generation and distribution system*: generates hydraulic power from engine-driven pumps and distributes hydraulic power to all connected systems.
