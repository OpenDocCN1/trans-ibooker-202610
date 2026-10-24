
be modelled and re-modelled without incurring excessive cost. Tools and techniques are available to allow this to happen. Alternative system designs can be compared and contrasted using simulation to see which best meets a specific requirement.

An additional advantage of producing robust and validated models is that they can be provided to other teams or to suppliers for them to develop their own functions to mutually agreed interface and performance definitions.

Chapter 3 illustrated how the cost of change increases as the product advances through its lifecycle. One advantage of modelling is to increase confidence in the correctness of the solutions that make up the complete system so that the likelihood of change is diminished. In addition, the results obtained can be used as evidence that the design is compliant with the requirement. It can be an advantage to gather this evidence early in the lifecycle in order to gain the customer’s confidence that the design is maturing and converging to a solution that can be qualified.

Typical types of models used in systems engineering are:

- *Simple diagrammatic model*: the thought process or intellectual process in which a systems engineer ‘imagines’ or envisages the structure and behaviour of a system during the mental process of perception, intuition, and reasoning during the evolution of a system concept.
- *Simple scale model*: a physical, scaled representation of the system or the components of a system.
- *Mathematical model*: a simple model of system behaviour described as changes of state with defined probability.
- *Simulation*: most complex, real-world systems with stochastic elements cannot be accurately described by a mathematical model that can be evaluated analytically. Thus a simulation is often the only type of investigation possible. Simulation allows one to estimate the performance of an existing system under some projected set of operating conditions.
- *Test rig*: a test facility that mimics part or the whole of the product, allowing experiments to be conducted under instrumented conditions.
- *Prototype*: a full-scale representation of a product or system that can be tested exhaustively to establish performance before manufacture.

It is often not practical to attempt to model a product in its entirety, and it may not even be necessary. What is most useful is to model those aspects of a system that are least understood and whose incorrect operation later in the lifecycle may pose a significant cost or time risk. It is important to use models correctly and generate simple models: *Models don’t have to be an exact representation of reality to be useful. In fact making them less realistic generally makes them more useful as long as they still provide useful insights. A model that is as complex as the thing it represents is likely to be too complex to be useful. A simple model is easier to work with.* (Stewart 2007).

Generating simple models can be accomplished by analysing the system and breaking it down into elements that can be modelled, and by understanding their interfaces and dependencies upon other elements of the system. Figure 7.7 shows how a number of different modelling techniques can be used in a complex system.

![](img/68b1a140c4426ceff0475b2d902d98df_215_0.png)

Figure 7.7 Example of the use of different modelling techniques.

Using different techniques in this way allows each function to be tested and refined as necessary until its performance is judged to be satisfactory. The results of each tested function can then be assembled manually to determine whether or not the combination also performs satisfactorily. An alternative, and better, method would be for the different models to be assembled into a framework that enables them to be connected to form a complete, or near-complete, simulation of the system. An illustration of the interconnection of models is shown in Figure 7.8, with an example of application to a fuel system.

The tank model in this example is important because it is used to translate the level of fuel determined by a number of probes under all conditions of roll and pitch, and translates this into fuel volume and then mass. This model application then in theory enables a fuel model to be designed which could replace a physical fuel rig. In fact the Nimrod MRA4 fuel system was qualified in this way.

This model can be extrapolated to show the complexity of a modern fuel system, as illustrated in Figure 7.9. The fuel system model must be designed to represent the following sub-systems and their interactions to complete a total simulation of a fuel system:

- A1 is the fuel system, the collection of tanks, fuel gauge probes, pumps, and valves that measure the quantity of the fuel in the tanks and ensure that it is moved from tank to tank and to the engine under the control of the fuel management section of the vehicle management system. For accurate measurement of fuel quantity the properties of fuel must be understood since fuel is known to stratify in the tanks in layers of density and temperature.
- A2 is the action of the aircraft in terms of pitch, roll and yaw position, and rates of change. This can cause the fuel attitude to change, thereby demanding careful selection

## Fuel system example

Figure 7.8 Examples of interconnected models.

![](img/68b1a140c4426ceff0475b2d902d98df_216_0.png)

Figure 7.9 Example of an interconnected fuel model.

![](img/68b1a140c4426ceff0475b2d902d98df_216_1.png)

gauging probe position to maintain accuracy, and also causing the fuel to slosh in large tanks. A model of this can be used to select the optimum position of baffles or expansion tanks.

- **B** represents the demand of the engine in terms of fuel flow throughout the flight envelope, and the fuel dump and expansion requirements.
- **C** represents the avionics integration. The flight control system (FCS) requires the fuel centre of gravity to be maintained within a predetermined range for either manoeuvre or fuel economy. The flight management system (FMS) needs to know the fuel quantity on board, whilst the flight deck displays will provide the crew with information about the fuel quantity in each tank, the status of the fuel system components, and the fuel remaining.
- **D** represents the heat exchangers provided to cool engine oil and hydraulic fluid in which large quantities of heat may be dumped into the fuel.
- **E** represents the mechanisms for uploading fuel on the ground or in the air, and for off-loading fuel in a controlled manner.
- **F** represents external factors such as ambient pressure and temperature. Temperature is an important factor since high ground ambient can cause expansion and spillage of fuel, whereas very low temperatures can lead to freezing of fuel in long duration polar routes – a factor in at least one serious accident.

This demonstrates that a model of the fuel system is quite complicated and demands close collaboration between the owners of different sub-systems. There is a choice to be made between a physical fuel system rig or a combination of models and simulation, or even a combination of all three to obtain a clear understanding of the behaviour of a modern fuel system. The trade-off is often the cost of a physical fuel rig, the facility to house it and the costs of running it compared with the cost of a model and simulation. To many people the physical rig has real benefits – it can be observed and measured doing its job and it uses real aircraft components, against the virtual world of the model. A decision that has exercised many minds in recent projects is the subject of the trade-off process. The answer would seem to lie in the degree of fidelity required – how representative is the model of the real world? The physical rig can be built to be very close to the aircraft in terms of components, spatial layout, realistic tank shapes etc. However, the rig can only be moved within a limited range of attitudes in pitch and roll, and will certainly not replicate rates of change. The simulation can only approximate to real components but can change attitude in real time.

## 7.3.4.1 Modelling Techniques

The modelling techniques introduced above are described in more detail in the following sections.

### 7.3.4.1.1 Simple Diagrammatic Models

Simple models captured in a pictorial representation are part of an intellectual process of envisaging the very early stages of design. They are mainly contained in the head of the designer and are used to envisage and explain concepts mentally. This may be a mental visualisation of a shape, a process or a mathematical expression. In order to achieve a common basis for discussion, diagrammatic models are
