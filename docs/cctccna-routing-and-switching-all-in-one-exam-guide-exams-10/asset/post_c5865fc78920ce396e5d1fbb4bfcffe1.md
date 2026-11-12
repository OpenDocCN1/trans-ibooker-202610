
## Ansible

Ansible is a little different from Puppet and Chef because it is agentless. You do not need to install an agent on the device because Ansible uses a push model, where the master (the controller) pushes the configuration change to the device. Ansible is a Python tool that uses the YAML language to create a *playbook*, which is the file that contains the instructions that need to be executed on the device after it is pushed to the device by the master. It is important to know for the exam that Ansible uses SSH to communicate with the device and apply the configuration, so SSH needs to be configured on the devices.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1055_0.png)

### EXAM TIP

For the CCNA exam, be familiar with the differences between Puppet, Chef, and Ansible. Also remember when the north-bound API is used and when the south-bound API is used.

## Chapter Review

In this chapter you learned about network configuration concepts such as traditional network management versus controller-based networking used for automation. You learned about software-defined networking and its architecture. You also learned how SDN separates the control plane from the data plane.

You learned about the Cisco DNA Center and the use of APIs to exchange data between components. You learned about the importance of JSON as a data format, with an example of the JSON data format. And, finally, you learned about configuration management technologies Puppet, Chef, and Ansible.

## Quick Review

### Introduction to Automation

- Traditional network management involves applying a configuration change to each device one at a time. This is time-consuming and prone to administration errors.
- Controller-based networking involves using automation technologies to send a change to the controller and have it apply the change to all devices simultaneously.
- The control plane is responsible for running routing and features to collect information to populate into tables used by the device.
- The data plane, also known as the forwarding plane, uses the information in tables to make forwarding or routing decisions.
- The management plane handles management access and AAA functions of authentication, authorization, and accounting.

### Software-Defined Networking

- In SDN, software is used to apply configuration changes to devices on the network.
- The SDN controller applies the configuration changes to devices on the network.
- There are three components to the SDN architecture: the underlay network, which is the network devices with protocols and features configured such as IP addresses and routing protocols; the overlay network, which is the virtual networks that are created in software; and the SDN fabric, which is all network devices and the SDN controller.

### Campus Device Management vs. Cisco DNA Center

- A Cisco DNA Center is an appliance you purchase that contains the SDN controller for your network.
- The north-bound API is used to send the configuration information from the application to the controller.
- The south-bound API is used to send the configuration information from the controller to the device.

### Exchanging Data

- The JSON data format is used to exchange information between environments.
- The REST API is the application logic that executes tasks over HTTP.
- The REST API uses HTTP verbs to perform operations.

## Questions

The following questions will help you measure your understanding of the material presented in this chapter. Read all the choices carefully because there may be more than one correct answer. Choose the correct answer(s) for each question.

- 1. Which of the following is a valid format for JSON data?
  - A. "empid":"126","name":"Bob Smith","phone":"555-5555"
  - B. {"empid":"126","name":"Bob Smith","phone":"555-5555"}
  - C. empid: "126",name: "Bob Smith",phone: "555-5555"
  - D. {empid: "126",name: "Bob Smith",phone: "555-5555"}
- 2. What API is used to communicate configuration information between the controller and the network device?
  - A. REST API
  - B. Web API
  - C. South-bound API
  - D. North-bound API
- 3. What component within the software-defined networking architecture contains the virtual networks?
  - A. South-bound API
  - B. Underlay network
  - C. Overlay network
  - D. Puppet
- 4. Which of the following are characteristics of traditional network management? (Choose three.)
  - **A.** The administrator focuses on the device.
  - **B.** The administrator focuses on the network.
  - **C.** The administrator inputs the configuration manually.
  - **D.** The administrator places configuration in policies.
  - **E.** It uses a whitelisting security model.
  - **F.** It uses a blacklisting security model.
- 5. Which configuration management application uses YAML as the scripting language?
  - **A.** Ansible
  - **B.** Puppet
  - **C.** Chef
  - **D.** Cisco DNA Center
- 6. Which of the following are characteristics of controller-based networking? (Choose three.)
  - **A.** The administrator focuses on the device.
  - **B.** The administrator focuses on the network.
  - **C.** The administrator inputs the configuration manually.
  - **D.** The administrator places configuration in policies.
  - **E.** It uses a whitelisting security model.
  - **F.** It uses a blacklisting security model.

## Performance-based Question

The Cisco exams have performance-based questions in which you must drag a box from the left side of the screen to its proper place on the right side of the screen to answer the questions. The following is a sample performance-based question related to this chapter:

1. Using the following exhibit, write each of the characteristics listed on the left side of the screen into the correct category on the right.

- Communicates using TCP port 8140
- Communicates using SSH
- Communicates using TCP port 10002
- Uses Ruby as a language
- Stores configuration elements in a manifest file
- Uses YAML as the configuration language

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1059_0.png)

## Answers

- 1. **B.** Each record within the JSON format is contained within curly brackets, { }. Each label is in quotes, followed by a colon, and followed by the value representing the data in quotes. Therefore, {"empid":"126","name":"Bob Smith","phone":"555-5555"} is correct.
- 2. **C.** The south-bound API is used to communicate configuration changes between the controller and the network device.
- 3. **C.** The overlay network contains the virtual network components for the SDN architecture.
- 4. **A, C, F.** With traditional network management, the administrator is focused on each individual device and connects to each device individually to apply changes manually. Traditional networking also uses a blacklisting model, where everything is allowed, and the administrator must block unwanted traffic.
- 5. **A.** Ansible uses YAML as the scripting language.
- 6. **B, D, E.** With controller-based networking, the administrator focuses on the network and making changes throughout the network at one time. This involves placing the configuration change into a policy element, and then applying the policy element to the network devices that need that change. Controller-based networking follows a whitelisting security model, in which everything is blocked until the administrator allows it.

## Performance-based Answer

The following are the answers to the performance-based question for this chapter. Remember that on the actual exam, you will need to drag and drop items to place them in their correct locations.

- 1. The following shows the correct mapping of characteristics for Puppet, Chef, and Ansible.

![](img/0eafdcebe219ef3756f0e57bcf1eb997_1060_0.png)
