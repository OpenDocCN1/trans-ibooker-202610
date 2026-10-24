
## 5.2 Definitions

The terms ‘architecture’ or ‘system architecture’ are much used in systems engineering. These terms owe much to their origin in civil engineering and building design. When systems engineers speak of the architecture of a system they do so in pretty much the same way as an architect speaks of the concept of a building. In civil engineering terms architecture is defined in the *Oxford English Dictionary* as:

- *The art and science of designing and supervising the construction of buildings.*
- *A style of building or structure.*
- *Buildings or structures collectively.*
- *The structure or design of anything.*

The *Oxford English Dictionary* defines architecture as *The special method or style in accordance with which the details of the structure and ornamentation of a building are arranged.* An architect is defined as *One who so plans, devises, contrives or constructs so as to achieve a desired result.* The architect envisages the form and structure of a design, often from a blank sheet of paper. He then flows down the guiding principles and standards that apply to the basic structure into the individual components. This ensures that the integrity of the design is preserved throughout the development of the product. This integrity includes aesthetic qualities such as style or fashion, as much as functional aspects such as habitation, heating, services etc. and adherence to regulatory aspects such as building regulations, health and safety, and all legal considerations.

This appreciation of pattern, form, and structure has uses beyond civil architecture. The decipherment of an ancient language (Linear B) was achieved by an architect, not a linguist, which led to an observation on the characteristics or competence of an architect:

> *The architect’s eye sees in a building not the mere façade, a jumble of ornamental and structural features; it looks beneath the appearances and distinguishes the significant parts of the pattern, the structural elements and framework of the building.*

> (Chadwick 1987)

It was this ability to observe pattern rather than detail that led the architect to conclude that the language represented by Linear B was Greek. Linguists had embroiled themselves in detail and in philosophical debate, and had missed the key point.

The above definitions and observations lead to the conclusion that form, structure, and order are essential characteristics of an architecture, rather than detail. An architect must therefore possess skills to deal with these characteristics and to develop a form and the standards that apply so that the constructors that follow his design can produce a sound structure. In aircraft systems engineering the fundamental architectural principles flow down through layers of system design into the very items of equipment that make up the hardware and firmware solution. This is illustrated in Figure 5.1.

For the purposes of this book the term ‘architecture’ has been used to identify a part of the design process that can be used to provide a preliminary pictorial description of a system. It is of most use in the early stages of the design lifecycle, especially the concept phase and the definition phase, but it remains a sound mechanism for recording the progress of design in the remaining stages of the process. It is especially useful for illustrating marketing material and in presentation as a clear and illustrative shorthand to support system descriptions.

Figure 5.1 Flow down of architectural principles.

![](img/68b1a140c4426ceff0475b2d902d98df_136_0.png)

In concept and design phases the architecture is used to introduce and develop initial concept design and establish interfaces and relationships with other systems. The architecture has a simplicity and clarity that stimulates discussion with other parties or stakeholders in the project. The architecture develops in stages, with agreement at each stage, until the design phase, where it is necessary to start to define the product to a stage where manufacturing can begin. This is where simplicity and clarity are subsumed in the level of detail required for specification and build.

## 5.3 System Architectures

In systems engineering at the early stages of design it is most convenient to think of form or structure rather than detailed engineering solutions. It is at this level of abstraction where decisions can be made about the major functional building blocks that are required and the means of communication between these blocks. For example, in computing systems the computer architecture is defined as the design and structure of the hardware components of computer systems. The term embraces general considerations, such as whether a system is based on serial, parallel, or distributed computing, in which several computers are linked together. It also covers more detailed aspects, such as a description of the internal structure of a central processing unit (CPU). A micro-computer is often described as having a 16-bit, 32-bit or 64-bit architecture according to the length of data word that can be processed by the CPU and the width of the data bus.

Similarly, when designing systems, engineers often speak of functions, processing standards, interface standards, software languages, and standards of data bus to connect the functions.

Once agreed at this level the principles are then applied at each succeeding level of detailed design by all parties involved.

A system architecture often starts life as a simple block diagrammatic representation of a system, a block diagram. This allows one to visualise the main functions to be performed, the mechanism for data interchange, and interdependencies between functions. When basic rules have been established for naming the major high-level functions and standards for data interchange, then blocks of the architecture can be developed in more detail. This is somewhat analogous to defining the ground plan and outline of a building, and agreeing standards to be observed for construction and provision of services. Figure 5.2 illustrates this process, showing the flow down of architectural principles in a generic system architecture.

Figure 5.2 Example generic system architecture.

![](img/68b1a140c4426ceff0475b2d902d98df_137_0.png)

The engineer who undertakes to control or set the standards for the architecture is known as a system architect. An architect is someone who practices the skills required to design a building and can be said to look for simple representations rather than detailed design. The intention of such simple representations is to create a medium that expresses high-level views of the design in a form that is simple and clear. This can be used as a means of stimulating debate, reaching agreement, and recording the stages of a design that all parties in a project can use as a sound basis for their work.

An example of a starting point for a total system architecture is shown in Figure 5.3. In this diagram the aircraft systems have been allocated to specific groups with a common means of intercommunication:

Figure 5.3 Example of a top-level system architecture.

![](img/68b1a140c4426ceff0475b2d902d98df_138_0.png)

- vehicle systems
- avionic systems
- mission systems
- cabin systems
- data bus.

A brief description of the functions is provided in each group. The adoption of a common data bus standard means that interfaces and data formats can be defined. The grouping is not arbitrary – the requirements for integrity for each group differ greatly. Vehicle systems are usually safety critical and must be designed so that failure is remote since it may endanger the aircraft and crew. Avionic systems are safety involved: their loss may hazard the aircraft. Mission systems failure will result in reduced performance or a failure to achieve the desired mission success rate. Loss of cabin and entertainment systems can be tolerated from a safety point of view, although customer satisfaction may diminish.

Although this is a very simple starting point it is an enabler for various teams to identify their responsibilities and their approach to design, and to further define the architectures in more detail. It is at this stage that standards for communication, safety, integrity, availability, and design for manufacture are established that will be applicable to all subsequent levels.

Figure 5.4 shows the architecture as it may look after each group has been developed further.

### 5.3.1 Vehicle Systems

The vehicle systems group has been developed to show the individual systems that will be required. The majority of such systems have a major mechanical content, such as pumps, tanks, levers etc., but it is quite valid to show the need for the systems in this architecture. Since vehicle systems need an element of control and monitoring this has also been included. To communicate with the rest of the aircraft a data bus connection is needed, since this is the agreed form of intercommunication. However, vehicle systems are vital to the continued safe operation of the aircraft, and to ensure that this robustness is maintained.
