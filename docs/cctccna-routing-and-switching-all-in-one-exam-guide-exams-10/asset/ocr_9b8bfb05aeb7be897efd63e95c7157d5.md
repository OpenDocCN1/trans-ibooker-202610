changes. Chef uses Ruby as a scripting language as well, with network communication using TCP port 10002.

# Ansible

Ansible is a little different from Puppet and Chef because it is agentless. You do not need to install an agent on the device because Ansible uses a push model, where the master (the controller) pushes the configuration change to the device. Ansible is a Python tool that uses the YAML language to create a *playbook*, which is the file that contains the instructions that need to be executed on the device after it is pushed to the device by the master. It is important to know for the exam that Ansible uses SSH to communicate with the device and apply the configuration, so SSH needs to be configured on the devices.

![](bbox=[0.14, 0.42, 0.24, 0.48])

**EXAM TIP** For the CCNA exam, be familiar with the differences between Puppet, Chef, and Ansible. Also remember when the north-bound API is used and when the south-bound API is used.

# Chapter Review

In this chapter you learned about network configuration concepts such as traditional network management versus controller-based networking used for automation. You learned about software-defined networking and its architecture. You also learned how SDN separates the control plane from the data plane.

You learned about the Cisco DNA Center and the use of APIs to exchange data between components. You learned about the importance of JSON as a data format, with an example of the JSON data format. And, finally, you learned about configuration management technologies Puppet, Chef, and Ansible.