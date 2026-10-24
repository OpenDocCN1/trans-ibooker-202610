
- systems architectures
- software requirements and specifications
- interface control documents (ICDs)
- wiring diagrams
- installation drawings or 3D models
- test procedures
- test results
- safety analyses
- statements of design
- equipment lists
- product build standards
- clearance standards
- plans such as engineering plans, management plans, project plans, etc.

These documents will be accompanied by concessions to record and authorise deviations from the baseline or to record shortfalls in performance. These concessions may lead to significant change or the need for re-design. This is usually authorised by a change authority. This allows the documentation set to be raised in issue to incorporate the changes into a new baseline design.

Any other systems or processes affected by the change are thereby informed and authorised to implement their own changes to ensure compatibility. This process ensures that change is understood by all stakeholders and that the product remains fit for purpose. In other words, the right part number of equipment (defining hardware and software) comes together with the right wiring and installation in the right standard of product.

## 9.3 A Simple Portrayal of a System

A classic closed-loop control loop is usually portrayed in its simplest form as shown in Figure 9.1. A system demand is fed into the forward path, which embodies and executes the control laws associated with the operation of the system and results in a system output. In a closed-loop system, the system output is fed back and compared with the system input

![](img/68b1a140c4426ceff0475b2d902d98df_270_0.png)

Figure 9.1 Classic closed-loop control system portrayal.

It can therefore be seen that the consideration of the physical boundary of the system immediately raises a number of additional considerations that affect system design and its impact on other surrounding systems. The diagram in Figure 9.2 is not sufficiently detailed to examine these issues and further information needs to be considered. It is also clear that as a system is initially developed before entering service, or evolves into different forms during its useful lifetime, these physical considerations will become ever more important. Examples illustrating this theme are described in the following section.

## 9.4 Varying System Configurations

In this discussion the issues surrounding the system physical issues are examined as well as those that affect system modification or evolution. The example given describes a notional system as it evolves from an initial configuration, System A, to a modified system, System B, and finally to a third variant, System C. As well as examining the hardware controller or ‘black box’ change issues, other equally important issues are addressed as follows:

- *System wiring:* In many systems the electrical wiring which interconnects input devices (controllers), output devices (actuators or effectors), and sensors is the most difficult item to change. On an aircraft, ship or motor vehicle the wiring harnesses are installed during vehicle build and assembly, and may be inaccessible once the vehicle is completed. Consequently, the wiring can be extremely difficult to change following build and this can be a significant constraint.

- *System software:* Many control systems developed today use ‘intelligence’ to achieve and maintain adequate system performance. This may involve the use of computers, microprocessors or micro-controllers to host and execute the necessary sophisticated control laws. System performance alteration or modification may be effected by changing these control elements, but the manner in which these changes may be introduced or embodied in an existing system requires considerable care to be exercised.

### 9.4.1 System Configuration A

System configuration A is shown in Figure 9.3. This diagram has been expanded to show additional features which relate to the description of the system. The additional items identified on this drawing include the following:

- An input device or demand, in this case shown as a lever, by which an operator may introduce a demand into the system. This input device is assumed to be connected to the controller by means of two wires.

- An output device or actuator that provides the ‘muscle’ to move a control surface or other effector such that the system will satisfy the operator’s demand. This actuator is assumed to be connected via four wires such that actuator demand and feedback signals may be exchanged.

- A controller, Unit 1, that closes the control loop for the system. This controller is assumed to have a time-variant control law F₁(t) in the forward control path and a fixed gain K1 in the feedback path.

![](img/68b1a140c4426ceff0475b2d902d98df_273_0.png)

**Figure 9.3** System configuration A.

- Sensors measuring parameters in the outside world or in other systems are used to modify the real-time performance of the forward path control function F₁(t) and are shown as four-wire sensors S₁ and S₂.
- A system software load, termed software load 1, is assumed to be loaded into the computing device that is executing the software associated with the F₁(t) function.

This diagram effectively encompasses and describes, in a shorthand fashion, all those elements necessary to achieve full and complete system operation. During the system development phase design effort will be concentrated on ensuring that all these items, hardware, software, and wiring, are developed together synchronously and tested to verify compliance with the original system specification.

### 9.4.2 System Configuration B

System configuration B is shown in Figure 9.4. If a system is developed or evolves from the original configuration, changes may be made to enhance performance, increase reliability, and offer other benefits and improvements. System B differs from the original configuration A as follows:

- The forward control path control laws have been modified from F₁(t) to F₂(t) though the sensors required, S1 and S2, are unchanged. The software load to accommodate the change in control laws is embodied in software load 2 and this software must be downloaded to implement the new control laws.
- As part of an improvement in the performance of the feedback path new time-variant control laws, K₂(t) and a new sensor S3 have been added. The changes to the feedback path control laws are embodied within the software load 2.
- The effect of these modifications in hardware and wiring terms has been to add another four-wire electrical input to Unit 2 to accommodate the input from new sensor S3. This may require an additional connector, which changes the hardware configuration.

![](img/68b1a140c4426ceff0475b2d902d98df_274_0.png)

Figure 9.4 System configuration B.

- Systems A and B now have incompatible hardware as the latter needs an extra four wires to interface with sensor 3 and possibly an additional connector. In the present form Units 1 and 2, associated with systems A and B, respectively, would not be interchangeable. For a customer using both variants this would result in an additional support overhead as both variants would need to be maintained in terms of spares, technical manuals, technician training etc.

### 9.4.3 System Configuration C

A final system, system configuration C, shown in Figure 9.5, is considered. In this system the following is assumed:

- The control laws are modified to F₃(t) in the forward path and K₃(t) in the feedback path. The further modified control laws associated with this implementation are embodied in software load 3.
- The improved control laws associated with software load 3 have negated the continued use of sensor 2 in the forward path. This sensor is no longer required for correct system operation and may therefore be removed.
- Unit 3 can be seen to have the same hardware configuration as Unit 2 in system B with the exception that sensor 2 is not connected. Units 2 and 3 may be interchangeable provided the correct software load, load 2 in the case of Unit 2 and load 3 in the case of Unit 3, is also present. For an operator utilising both system configurations B and C this may result in a significant reduction in support overhead.

A summary of the three system configurations is given in Table 9.1.
