![](bbox=[0.12, 0.12, 0.86, 0.42])

Figure 5.3 Example of a top-level system architecture.

A brief description of the functions is provided in each group. The adoption of a common data bus standard means that interfaces and data formats can be defined. The grouping is not arbitrary – the requirements for integrity for each group differ greatly. Vehicle systems are usually safety critical and must be designed so that failure is remote since it may endanger the aircraft and crew. Avionic systems are safety involved: their loss may hazard the aircraft. Mission systems failure will result in reduced performance or a failure to achieve the desired mission success rate. Loss of cabin and entertainment systems can be tolerated from a safety point of view, although customer satisfaction may diminish.

Although this is a very simple starting point it is an enabler for various teams to identify their responsibilities and their approach to design, and to further define the architectures in more detail. It is at this stage that standards for communication, safety, integrity, availability, and design for manufacture are established that will be applicable to all subsequent levels.

Figure 5.4 shows the architecture as it may look after each group has been developed further.

# 5.3.1 Vehicle Systems

The vehicle systems group has been developed to show the individual systems that will be required. The majority of such systems have a major mechanical content, such as pumps, tanks, levers etc., but it is quite valid to show the need for the systems in this architecture. Since vehicle systems need an element of control and monitoring this has also been included. To communicate with the rest of the aircraft a data bus connection is needed, since this is the agreed form of intercommunication. However, vehicle systems are vital to the continued safe operation of the aircraft, and to ensure that this robustness is maintained