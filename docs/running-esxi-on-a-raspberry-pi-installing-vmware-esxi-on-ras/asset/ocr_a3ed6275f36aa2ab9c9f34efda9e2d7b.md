in an earlier chapter, Horizon desktops running on Arm are not supported, but we can still look at the Arm-based ESXi hosts and the VMs that run on them.

ControlUp Real-Time Console, like many third-party products for VMware, comes with a free trial license. It took me about five minutes to download and install the console on a Windows machine. I then added vCenter Server that was associated with my ESXi on Arm server to the console. Within moments, I saw the server and the VMs that were running on it.

![](bbox=[0.125, 0.343, 0.9, 0.66])

ControlUp obtains information about the VMs from vCenter Server, but one of its unique features is that it can also obtain information about what is happening inside a VM. It uses an agent to get this information