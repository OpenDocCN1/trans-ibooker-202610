

![](img/68b1a140c4426ceff0475b2d902d98df_304_0.png)

**Figure 10.15** The aircraft requires multiple sources of aircraft attitude information to provide the pilot with attitude guidance and to support the needs of many subsystems. Normally primary, secondary, and standby sources of attitude are provided: primary attitude source (inertial navigation system or air data and inertial reference system), secondary attitude source (attitude and heading reference system), standby source of attitude to stand-alone backup instruments.

## 10.7.3 Attitude Measurement System

The aircraft requires pitch, roll, and yaw attitude information in order to be flown safely. Attitude systems are required to display pitch and roll information on the primary flight display (PFD) and yaw/heading information on the navigation display (ND), which both pilots have as part of their electronic flight instrument system.

In addition, three-axis velocity and acceleration data relating to aircraft motion are required for the FBW and autopilot systems. Usually aircraft have primary and secondary attitude systems with some form of standby system, as illustrated in Figure 10.15; the exact implementation depends on the system architecture and the technology employed. On most aircraft the inertial reference system (IRS) provides the primary attitude source whilst a secondary attitude and heading reference system (AHRS) yields an alternative source of attitude information.

In recent years improved technology using micro-inertial sensing devices and packaging techniques has resulted in small 2ATI instruments providing an integrated standby instrument system (ISIS) that provides an independent source of attitude information to the pilot in the event that the primary and secondary sources are not available.

## 10.7.4 Air Data System

The air data relating to the aircraft speed and height whilst passing through the air is of critical importance. A combination of pitot and static probes senses the total and static pressure as the aircraft moves through the air as a fluid medium. In the simplest form this information can be combined to derive airspeed, altitude, and rate of climb and descent. The addition of other sensors, such as total air temperature probes and air-stream direction detectors, allow even more useful derived data to be calculated. By using the digital computing capability, air data computers (ADCs) or air data modules (ADMs) are able to calculate other more useful parameters, such as indicated airspeed (IAS), true airspeed (TAS), Mach etc.

Due to the level of criticality of air data it is usual to have three independent channels with a standby channel, as illustrated in Figure 10.16. In the past, the standby channel has been composed of dedicated small standby instruments fed by their own dedicated pitot-static system. The availability of the ISIS technology described above now means that two multi-function, solid-state backup instruments can be used in the place of three dedicated mechanical instruments. The reduction in the number of standby instruments is enabled in part due to the increase in reliability by changing to solid state instruments rather than conventional instruments as well as the multi-function display capability of the newer instruments.

![](img/68b1a140c4426ceff0475b2d902d98df_305_0.png)

Figure 10.16 The availability of air data is critical for safe flight. Air data computers or air data modules provide triple redundant sources of pitot and static air data information. Calculations convert basic pitot and static pressure into more meaningful parameters such as indicated air speed, true air speed, Mach number, barometric altitude etc. Air data are extensively used by many aircraft systems. Backup data are provided to standby systems.

## 10.7.5 Electrical Power System

As has been mentioned earlier, the electrical power system is a key system. Most, if not all, aircraft systems depend upon the supply of electrical power either as 115VAC 3-phase or 28VDC supplies derived from transformer rectifier units (TRUs).

A common electrical power generation configuration employed is to have three generators, one driven by each of the aircraft engines and a third driven by the aircraft auxiliary power unit (APU). The original purpose of installing the APU was to provide a source of electrical power and medium pressure air whilst the aircraft was on the ground prior to departure. The APU could also be used as a source of pneumatic power to start the aircraft engines. In many configurations in service today, the APU can be used to provide a third electrical power generation source by starting the APU in flight. In some systems this can be accomplished at cruising altitude in excess of 30 000 ft; in others the aircraft has to descend to medium altitude – say 20 000 ft – in order for the APU to be in the start envelope. Still other systems run the APU continuously during the cruise when operating in an extended twin operations (ETOPS) configuration, that is operating a twin-engine aircraft such as a Boeing 737 over 60 minutes away from a diversion airfield. Special considerations apply to ETOPS operation (see Advisory Circular AC 120-42A 2008).

Due to the importance of electrical power a further backup power system is provided. In smaller aircraft this may be provided by sizing the aircraft batteries such that they can provide adequate power for short-term use (up to 30 minutes) in the event that primary and secondary sources are lost. Other aircraft rely on the use of a RAT to provide the aircraft with an emergency power source. The RAT is an air-driven turbine normally stowed within the aircraft fuselage. When required, it is released into the airstream. It is rotated by the airflow and drives a small embedded electrical generator. Once deployed the RAT cannot be re-stowed in flight and must be reset by a maintenance action once the aircraft has landed. As will be seen, the RAT can also be used as a source of emergency hydraulic power. This is illustrated in Figure 10.17.

![](img/68b1a140c4426ceff0475b2d902d98df_306_0.png)

Figure 10.17 Aircraft usually possess three independent channels of electrical power: left main generator, right main generator, and auxiliary power unit generator. A battery is also provided which can provide a short- to medium-term alternative energy source. An emergency source such as a ram air turbine supplies power when all other power is lost.

## 10.7.6 Hydraulic Power System

The need for hydraulic power to provide the muscle to drive the flight control and other hydraulic systems has already been described. Normally hydraulic power is derived from EDPs mounted on the engine accessory gearbox. However, hydraulic pumps may also be electrically driven by either AC or DC electrical motors. On Boeing’s wide-body aircraft – the Boeing 747/767/777 – some hydraulic pumps are air driven. The reason for using these dissimilar means of generating hydraulic power is to achieve additional levels of segregation and redundancy as well as meeting diverse system demand (flow rate) conditions.

A typical aircraft will have three independent hydraulic systems driven by a combination of the aircraft EDPs, electrically driven pumps, and/or air-driven pumps (ADPs). These systems will be isolated such that a major system failure, such as total loss of hydraulic fluid within one system, does not cascade from one hydraulic system into another. Some systems use units called power transfer units (PTUs) which can transfer energy from one system to another whilst at the same time preserving segregation between them.

As has been mentioned, another source of hydraulic power can be provided by a RAT driving a dedicated hydraulic pump to provide a short-term source of power in an emergency situation. This is illustrated in Figure 10.18.

![](img/68b1a140c4426ceff0475b2d902d98df_306_1.png)

Figure 10.18 Usually three independent channels of hydraulic power are provided. Hydraulic power is derived from engine-driven pumps, AC motor pumps, and in some cases air-driven pumps. Hydraulic accumulators provide a short-term backup. Emergency power is supplied from a ram air turbine.

