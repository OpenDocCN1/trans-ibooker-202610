
![](img/68b1a140c4426ceff0475b2d902d98df_229_0.png)

Figure 7.16 An 'iron bird' rig.

on YouTube). For fast jet military aircraft a ground rig will use a rocket-powered slide to simulate high-speed conditions and an ejection seat equipped with a realistic size and weight dummy will be used with high-speed video to demonstrate safe ejection.

- Iron bird: a combination of hydraulics power, flying controls, and landing gear that allows full movement of all surfaces and components to check range, rate, and freedom of movement. All the equipment required to provide control and demand signals, e.g. flight control computers and landing gear computers, will be mounted on the test bench and wired up to sub-systems such as flight control surfaces with actuators providing realistic flight loads. A source of hydraulic power can be provided from a ground truck or from a test rig operating an aircraft standard hydraulic system. A block diagram of an iron bird rig is shown in Figure 7.16.

## 7.3.8 Aircraft Ground Testing

There will come a time when all the individual systems have been tested and the aircraft prototype will have been assembled and all wiring installed. The wiring is tested for end-to-end conductivity and resistance, and an insulation test is conducted (Google 'DITMCO for products'). It is usual to install a ground electrical system and power this up to provide suitable aircraft power supplies before installing and connecting other equipment.

This enables the aircraft connections to be tested to ensure the correct electrical power and ground connections have been wired to the equipment connectors. Voltage, polarity, insulation resistance, and earth conductance will be measured. When this is complete it is safe to install the aircraft equipment and start to test the systems individually to agreed test procedures. Any defects or failures to meet test procedures will be signalled to design for appropriate rectification action.

This continues until all tests are complete and the aircraft is cleared to move onto ground runs with engines providing power for the electrical and hydraulic generation systems. Once these tests are complete the aircraft is cleared for engine runs, taxi trials, and prototype flying.

## 7.3.9 Flight Test

Following satisfactory completion of ground testing all the evidence from testing is reviewed to obtain clearance to proceed to flight testing. This is often conducted on one or more prototypes to enable tests to be conducted in realistic flight and environmental conditions. For more information read the series of books by Longworth (2012, 2013, 2014).

Some examples of test flying of prototypes and production aircraft are described below.

- *Prototype flight testing:* In order to eliminate risk from the series production programme many product manufacturers produce one or more prototypes which are subjected to a rigorous test programme to demonstrate correct performance. Full-scale prototypes are an extremely costly form of model, but the penalty of proceeding to series production without fully understanding the performance issues may be even more costly. The aerospace industry has long made use of prototypes to explore the full flight envelope and to demonstrate the performance of avionic systems. The results gained during such testing are fed back into the early models to improve their fidelity and to validate the model results. A model validated by flight test results establishes confidence in the model so that it can be used to support future changes and developments. The number of prototypes in any one programme is determined by the amount of testing to be done; the load of general and specialist systems tests is usually spread across the available aircraft to enable concurrent testing to take place.

Figure 7.17 shows four different prototypes from a period of 50 years of history of the European aerospace industry. The P1A prototype was the first of many that were used to develop the handling characteristics and speed of the UK’s first supersonic production interceptor, the English Electric Lightning. The Lightning prototypes also demonstrated the capability of the aircraft as an integrated weapon system integrating the capabilities of the pilot, the AI 23 radar, a missile, and the aircraft avionics.

![](img/68b1a140c4426ceff0475b2d902d98df_231_0.png)

**Figure 7.17** Examples of prototypes: (a) English Electric P1B, (b) the Experimental Aircraft Programme (EAP), (c) Panavia Tornado prototype P01, and (d) Typhoon UK prototypes (photos: BAE Systems).

Tornado was designed as a multi-role combat aircraft (MRCA) by a grouping of three European aircraft companies, known as Panavia. Nine prototypes developed this swing-wing supersonic aircraft that led a successful service life. Tornado P01 is seen on its first flight from Manching in Germany on 14 August 1974. The type retired from RAF service in 2019 in its GR4 configuration but still remains in service with other users.

The experimental aircraft programme (EAP) was a single demonstrator aircraft to explore the handling of an aircraft designed to be aerodynamically unstable with a quadruple fly-by-wire system. Other technologies demonstrated included composite materials, a full-colour ‘glass’ cockpit, a computer-controlled set of vehicle systems, and a real-time processing system using a high-order language and the first flying MIL-STD-1553B data bus. After 259 flights of testing and demonstration flying this aircraft was retired as a teaching aid at Loughborough University. It was donated to the RAF Museum at Cosford in the UK in 2014.

Typhoon used much of the technology that was demonstrated on EAP and developed it further. Flight testing demonstrated the aircraft’s handling and its comprehensive systems and armaments capability, performed by pilots of all the participating nations. The single-seat and trainer versions are shown in Figure 7.17. Some examples are described below.

- *Production acceptance flight testing:* A short production acceptance test is performed on each aircraft leaving the production line before it is handed over to the customer.

## 7.3.10 Trials

Trials are conducted by the customer’s specialist agencies to evaluate specific requirements and the ability of the product to meet those requirements. This may mean that the aircraft is tested on specific range assets such as radar range, weapon delivery range or in theatres of operation such as the tropics, the Arctic, and deserts. For commercial aircraft it may be necessary to evaluate the aircraft at new air terminals to demonstrate compatibility and the ability to board and disembark passengers.

## 7.3.11 Operational Test

Operational testing will be conducted when the aircraft is in service to meet new situations as and when they arise. This will allow the product to be progressively adopted in new scenarios.

## 7.3.12 Demonstrations

There are occasions when a demonstration is needed before final acceptance of some requirements is completed. Often this is for requirements which have statistical outcomes. Examples are reliability or availability during which operational information is collected over a period of time. Another example which requires the involvement of operational personnel is the demonstration of supportability.

## 7.4 An Example Using a Radar System

This section summarises the examples described above for a simple radar system to illustrate what the resulting documentation will look like. Figure 7.18 shows the block diagram of a radar system.

![](img/68b1a140c4426ceff0475b2d902d98df_232_0.png)

There are a number of requirements that flow down from the top-level aircraft requirement specification that determine the primary performance characteristics, for example detection range, target size capability, clutter discrimination, classification and identification of targets, and ability to track and lock on. There are also a number of derived requirements that arise from incorporating the radar into an aircraft. These are illustrated in Figure 7.19.

![](img/68b1a140c4426ceff0475b2d902d98df_233_0.png)

Figure 7.19 Derived requirements for a radar system.

These derived requirements arise from a need to design a radome that will include the swept volume of the dish, that is aerodynamically sound as well as strong (to withstand bird strike and driving rain), has low weight, can be securely attached yet easy to open for maintenance, and has the appropriate transmission characteristics so as not to attenuate the radar pulse and its return signal. There is a health concern to reduce the occurrence of any non-ionising radiation being transmitted into the cockpit from any side lobes. A plan for obtaining qualification information can be summarised in a verification cross-reference matrix, as shown in Figure 7.20. The matrix identifies the requirements that need to be tested and by what test mechanism, shown as shaded blocks. In a firm plan these blocks

| | Design | Calc | Analogy | Model | Test rig | Env test | Int test | Trials | Operational | Demo |
|---|---|---|---|---|---|---|---|---|---|---|
| Detection range |  |  |  |  |  |  |  |  |  |  |
| Target CSA |  |  |  |  |  |  |  |  |  |  |
| Clutter discrimination |  |  |  |  |  |  |  |  |  |  |
| Classification |  |  |  |  |  |  |  |  |  |  |
| Identification |  |  |  |  |  |  |  |  |  |  |
| Tracking |  |  |  |  |  |  |  |  |  |  |
| Lock-on |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |
| Mass |  |  |  |  |  |  |  |  |  |  |
| Power |  |  |  |  |  |  |  |  |  |  |
| Volume |  |  |  |  |  |  |  |  |  |  |
|  |  |  |  |  |  |  |  |  |  |  |
| Radome swept area |  |  |  |  |  |  |  |  |  |  |
| Transmissivity |  |  |  |  |  |  |  |  |  |  |
| Aerodynamics |  |  |  |  |  |  |  |  |  |  |
| Bird strike |  |  |  |  |  |  |  |  |  |  |
| Driving rain |  |  |  |  |  |  |  |  |  |  |
| Attachment strength |  |  |  |  |  |  |  |  |  |  |
| Lightning strike |  |  |  |  |  |  |  |  |  |  |
| RF energy SHE |  |  |  |  |  |  |  |  |  |  |
| Maintenance access |  |  |  |  |  |  |  |  |  |  |
| Integration |  |  |  |  |  |  |  |  |  |  |

Figure 7.20 Verification cross-reference matrix for a radar system.
