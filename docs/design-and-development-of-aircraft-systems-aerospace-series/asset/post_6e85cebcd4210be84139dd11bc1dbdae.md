
to be separated from the processing function to devices known as RIO. With developing technology it became possible for RIOs to be installed in parts of the aircraft that had a relatively harsh environment, but in remote areas that further reduced the lengths of wiring harnesses and reduced weight, whilst keeping the processing components in a cooled environment. Figure 5.35 shows how this separation was devised using a VMS architecture and Figure 5.36 shows a VMS architecture using two RIOs.

![](img/68b1a140c4426ceff0475b2d902d98df_169_0.png)

**Figure 5.35** Developing the RIO concept.

![](img/68b1a140c4426ceff0475b2d902d98df_169_1.png)

**Figure 5.36** Example VMS architecture using RIOs.

## Exercises

1 Design an architecture for a remotely piloted aircraft to deliver commercial items in a rural setting. Identify all essential sub-systems on board and identify the ground segment and suitable telemetry links.

2 Repeat the above exercise for an autonomous system.

3 Describe how you would prepare the evidence for certification of these systems.

4 Put yourself in the position of the engineer responsible for the data bus structure for a long-range commercial aircraft. Produce a diagram of your proposed data bus architecture and identify all the bus types you need. Define the key characteristics to enable users of your architecture to integrate to meet their system performance and to meet the safety and performance requirements of the aircraft.

5 Take into account the notion of a human being as an integrated system given in Chapter 6 and devise a system architecture of a human. How does the performance of this differ from an aircraft architecture?

6 Compare and contrast the architecture of a smart phone and an aircraft. Comment on the similarities and differences between the two architectures.

## References

Chadwick, J. (1987). *The Decipherment of Linear B.* Cambridge University Press.

Moir, I., Seabridge, A., and Jukes, M. (2013). *Civil Avionic Systems*, 2e. Wiley.

Pallet, E.H.J. (1987). *Aircraft Instruments & Integrated Systems.* Longman, ISBN: 0-582-08627-2[1].

Weller, B. (2018). *A History of the Fly-by-Wire Jaguar.* BAE Systems Heritage Department.

## Further Reading

Chau, S., Dang, V., Xu, J., and Lu, J. (2006). *An Automatic Technique to Synthesize Avionics Architecture.* *Proceedings of the First NASA/ESA Conference on Adaptive Hardware and Systems (AHS’06).* IEEE Computer Society, Washington, DC.

Maier, M.W. and Eberhardt, R. (2002). *The Art of Systems Architecting*, 2e. CRC Press.

Moir, I., Seabridge, A., and Jukes, M. (2013). *Civil Avionic Systems*, 2e. Wiley.

Salomon, U. and Reichel, R.. (2013). *Automatic Design of IMA Systems.* IEEEAC Paper #2018, Version 3, Updated 19/01/2013.

Seabridge, A. and Leon, S. (2016). *EAP – The Experimental Aircraft Programme: Britain’s Last Manned Aircraft Demonstrator.* BAE Systems Heritage Department.

Stevens, R., Brook, P., Jackson, K., and Arnold, S. (1998). *Systems Engineering – Coping with Complexity.* Prentice Hall.

## 6

## System Integration

### 6.1 Introduction

The term ‘system integration’ is understood to mean different things by different people and by many different organisations. This chapter will examine some aspects of system integration and offer the reader some of the potential down sides to encourage a level of caution and scrutiny of the design to ensure safe solutions. This is discussed further in Chapter 11.

Integration arises because engineers want to pursue solutions that are efficient in operation and in their use of equipment. To achieve this latter desire, engineers often seek to incorporate many functions into single devices such as hardware components, line replaceable items (LRIs) or software packages. The design drivers for this are to do with cost, weight, installation volume in the aircraft, reliability, and in some cases technological challenge. In addition to this most computing devices encourage multi-tasking solutions, as illustrated by personal computers.

Whilst the results have many tangible benefits, there are some drawbacks. Some integrated solutions appear on the surface to be simple and to offer a simple man–machine interface, rather like the Apple iPod. This is achieved by quite high levels of sophistication and complexity within the device. In a complex aircraft system encompassing all of the avionic and aircraft systems the result is hugely complex with interactions of hardware, software, data, and functions occurring throughout the system.

Perhaps the easiest way to grasp the concept is to provide an example of a familiar integrated system such as the human being.

The human being is a good example in terms of understanding system integration as it embodies all the attributes that an engineer would like to incorporate into a perfect system. An aircraft can be considered as a complex set of interacting sub-systems, not dissimilar to a human being. Indeed, with the emergence of unmanned air systems, and especially autonomous unmanned air systems, this ideal of an integrated system behaving with minimal operator intervention is fast becoming the target of future systems designers. Figure 6.1 shows the similarities that can be observed between the human being and an aircraft in systems terms.

The human being has a frame consisting of a skeleton with a surrounding structure. Into this structure are incorporated a source of energy converted from an appropriate fuel, a

![](img/68b1a140c4426ceff0475b2d902d98df_172_0.png)

Figure 6.1 Comparison of a human being and an aircraft as a system.

complex processing system, mechanisms for converting energy into movement, the ability to sense the conditions of the surrounding environment, mechanisms for reacting to and compensating for climatic conditions, and finally a means of undertaking purposeful activity or providing motive power. The human being can perform a number of complex tasks simultaneously:

- acquire and process information from various sensors
- think, analyse, calculate, judge
- perform vital functions – breathing, blood circulation, balance, movement, digestion, etc.
- react to information received in a purposeful manner
- react instinctively to external stimuli
- exercise moral and ethical considerations in decision making
- prioritise all these functions for maximum effectiveness.

This can be expressed as the merging or integration of individual sources of data, information, and knowledge to perform a function. The human being is an effective and innate integrator of information. For example, knowing where one is and where one needs to move to is a simple, intuitive function: Our sense of orientation is derived from integrating information from balance organs in the inner ears with that from muscle and joint receptors that signal the position of the limbs, together with visual cues (Ashcroft 2000). This ability to assimilate and integrate information and to react in many different ways is true for many human functions.

The human being, therefore, represents the successful interaction and integration of complex information acquisition and processing together with a means of energy transformation that enables it to achieve an outcome. This, together with the ability to compensate for changes in the physical environment and the ability to assess threats and react to them is a central attribute. This ability has assured the position of mankind as the ‘supreme’

animal in the global environment. This is also a key requirement of aircraft weapon systems. The aircraft that use of information from sensors, knowledge of the outside world, and transfer of energy into action must be processed by some form of computing system to mimic the intuitive processing of the human brain. Bringing together all these actions to achieve a desired outcome using mechanical and electrical/electronic means in a machine with a human operator is a key skill of system integration. It is the genetic influences and received wisdom that shape the intellectual development of the human being over a long period of evolution according to Darwin. An aircraft, on the other hand, must be designed to meet a particular set of requirements in a relatively short time period. A diverse range of skills and processes is needed to do this to produce a vehicle capable of achieving a wide range of activities in a diverse range of operating scenarios. This task is system integration.

### 6.2 Definitions

The term system integration can be interpreted in a number of ways, and the following interpretations are commonly used in the aircraft industry:

- *Integration at the component level:* The ability of a component or LRI to ensure that the discrete function it offers contributes to the overall system in which it resides.
- *Integration at the system level:* The merging of discrete functions and characteristics previously performed by discrete control items into common areas of control.
- *Integration at the process level:* The progressive build-up of product components into a single, working and tested product.
- *Integration at the functional level:* The identification of integrated functions that are an amalgamation of many individual functions to form a demonstrable measure of performance.
- *Integration at the information level:* The recording and authorisation of information to define, design, document, and certify fitness for purpose of the complete system.
- *Integration at the prime contract level:* the ability to design, develop and manufacture a complex product that precisely meets the customer’s requirement throughout the product lifecycle.
- *Integration from emergent properties:* a phenomenon of interactions between sub-systems that may not have been purposely designed but arise as a result of emergent properties of the constituent systems.

### 6.3 Examples of System Integration

An explanation with examples will provide an understanding of each of these instances of integration.

### 6.3.1 Integration at the Component Level

Integration at the component level is important as this provides the building blocks from which a sub-system or system is constructed. A number of electronic components, when
