It can therefore be seen that the consideration of the physical boundary of the system immediately raises a number of additional considerations that affect system design and its impact on other surrounding systems. The diagram in Figure 9.2 is not sufficiently detailed to examine these issues and further information needs to be considered. It is also clear that as a system is initially developed before entering service, or evolves into different forms during its useful lifetime, these physical considerations will become ever more important. Examples illustrating this theme are described in the following section.

# 9.4 Varying System Configurations

In this discussion the issues surrounding the system physical issues are examined as well as those that affect system modification or evolution. The example given describes a notional system as it evolves from an initial configuration, System A, to a modified system, System B, and finally to a third variant, System C. As well as examining the hardware controller or ‘black box’ change issues, other equally important issues are addressed as follows:

+   - *System wiring:* In many systems the electrical wiring which interconnects input devices (controllers), output devices (actuators or effectors), and sensors is the most difficult item to change. On an aircraft, ship or motor vehicle the wiring harnesses are installed during vehicle build and assembly, and may be inaccessible once the vehicle is completed. Consequently, the wiring can be extremely difficult to change following build and this can be a significant constraint.

+   - *System software:* Many control systems developed today use ‘intelligence’ to achieve and maintain adequate system performance. This may involve the use of computers, microprocessors or micro-controllers to host and execute the necessary sophisticated control laws. System performance alteration or modification may be effected by changing these control elements, but the manner in which these changes may be introduced or embodied in an existing system requires considerable care to be exercised.

# 9.4.1 System Configuration A

System configuration A is shown in Figure 9.3. This diagram has been expanded to show additional features which relate to the description of the system. The additional items identified on this drawing include the following:

+   - An input device or demand, in this case shown as a lever, by which an operator may introduce a demand into the system. This input device is assumed to be connected to the controller by means of two wires.

+   - An output device or actuator that provides the ‘muscle’ to move a control surface or other effector such that the system will satisfy the operator’s demand. This actuator is assumed to be connected via four wires such that actuator demand and feedback signals may be exchanged.

+   - A controller, Unit 1, that closes the control loop for the system. This controller is assumed to have a time-variant control law F₁(t) in the forward control path and a fixed gain K1 in the feedback path.