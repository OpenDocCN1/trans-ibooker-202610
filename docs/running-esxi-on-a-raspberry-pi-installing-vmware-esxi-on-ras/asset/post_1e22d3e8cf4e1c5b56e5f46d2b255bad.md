
In an earlier chapter, Horizon desktops running on Arm are not supported, but we can still look at the Arm-based ESXi hosts and the VMs that run on them.

ControlUp Real-Time Console, like many third-party products for VMware, comes with a free trial license. It took me about five minutes to download and install the console on a Windows machine. I then added vCenter Server that was associated with my ESXi on Arm server to the console. Within moments, I saw the server and the VMs that were running on it.

![](img/430161835ae267467687add4a1cdc429_359_0.png)

ControlUp obtains information about the VMs from vCenter Server, but one of its unique features is that it can also obtain information about what is happening inside a VM. It uses an agent to get this information from Windows machines and a Linux Data Collector (LDC) to obtain information from Linux systems. After setting up the LDC, I was able to see metrics from the VMs to gain deeper insight about them.

![](img/430161835ae267467687add4a1cdc429_360_0.png)

This included the processes that were running on the VMs.

![](img/430161835ae267467687add4a1cdc429_360_1.png)

In one case, I found that the stress level was red due to disk latency. Upon investigating the issue, I found that this was due to the thumb drive that it was using for storage.

![](img/430161835ae267467687add4a1cdc429_361_0.png)

## Chapter 13: Managing ESXi Systems with VCSA

When running iPerf3 between two VMs on the same host, I monitored the network statistics in real time using the ControlUp dashboard.

![](img/430161835ae267467687add4a1cdc429_362_0.png)

Although ControlUp was designed for VDI environments (i.e., virtual desktops and applications), it also monitors the infrastructure it is running on in order to give a holistic view of a VDI environment. After spending a couple weeks with the Arm-based ESXi hosts, I found that it could monitor everything on it as well as on my x64-based ESXi hosts.

### Summary

vCenter Server is an extremely powerful tool, and we gave it short shrift in this chapter to stay within the scope of this book. One of its major powers is the ability to create scripts to programmatically work with and manage them via PowerShell using PowerCLI. A huge and enthusiastic community has formed around this and has come up with scripts to automate just about any task.

Although I was able to use Player to install and run VCSA, it does consume a fair amount of resources (two vCPUs and 12GB of memory), so make sure the system that you are using is equipped to handle the load that will be put on it.

By using the VCSA and its vSphere Client, you can clone a VM with just a few clicks and vMotion VMs to/from different hosts and different storage even as they are running, as well as perform many, many other operations that we didn’t even touch on.

Most third-party products from the VMware ecosystem require vCenter in order to function. My tests with ControlUp Real-Time Console demonstrate that it can effectively monitor and manage the hypervisor and VMs that reside on it.

With vCenter Server installed and functional, you have the basis for a fully functional vSphere environment! From this environment, small as it is, you can begin to explore the power and capabilities of a virtualized environment and learn more about how it has enabled thousands of businesses great and small. In the next chapter, we will see how Arm is going to play with the datacenter of the future.

## Chapter 14: The Future of ESXi on Arm

In this book, we have demonstrated how to get started with VMware on Arm. The future goes well beyond using VMware on a Raspberry Pi (Figure 14-1). While the capability may be relatively new, it is crucial for VMware’s continued relevance in the market.

![](img/430161835ae267467687add4a1cdc429_364_0.png)

**Figure 14-1.** Raspberry Pi Supercomputer at Oracle OpenWorld 2019

The ecosystem for Arm servers is set to look a lot different from the traditional x86 servers that have dominated the past decade. Companies such as Amazon AWS with Graviton, Ampere with Altra, and Huawei with Kunpeng are bringing innovative servers competitive with the best x86 servers from Intel and AMD. What is more, instead of being lab projects of curiosity, the new processors are being adopted by major cloud providers. Today these processors make up a small fraction of the CPU cores that are used in the cloud, but they are steadily increasing.

Using Arm cores goes beyond general-purpose compute. While x86 has cleaved to legacy infrastructure dominated by generations of backward compatibility, Arm is starting fresh, tailoring its hardware to specific workloads such as cloud and high-performance computing (HPC). Indeed, in June 2020, supercomputer Fugaku became the world’s fastest supercomputer based on the semi-annual TOP500 supercomputer list published by TOP500.org. That accomplishment was due to custom Arm cores that are well tailored to the HPC market in a way that would be nearly impossible in the modern x86 market, with Intel’s Knights Landing and Knights Mill being the most recent examples. There are several other Arm for HPC projects underway as of mid-2021 in other geographies. NVIDIA has already announced its “Grace” solution years early, bringing an Arm CPU and GPU together (see Figure 14-2).

![](img/430161835ae267467687add4a1cdc429_366_0.png)

**Figure 14-2.** NVIDIA Grace Arm CPU and GPU as shared at NVIDIA GTC 2021

The next boom for Arm in the datacenter is by adding Arm as the infrastructure gateway to devices in the datacenter. Datacenter operators in modern environments face a fundamental challenge every day. Users that are running workloads may or may not be trustworthy. Even trustworthy users may download malicious code. As a result, datacenter operators are on a journey to separate the infrastructure and application layers in their environments. AWS Nitro was the leader in this space, but the rest of the industry is catching up.

Using a new class of device, currently called the DPU or data processing unit, datacenter operators are able to maintain a secure endpoint where services can be provisioned. Most current DPUs have Arm CPUs, memory, storage, high-speed networking, and security accelerators built in. They run their own OSs whether that is Linux like Ubuntu or, as with VMware’s Project Monterey, VMware ESXi directly on the PCIe add-in card. With the major cloud providers adopting this operating model, VMware needs to put ESXi on DPUs to bring their customers a similar level of capability. DPUs generally are Arm-based, which means that without Arm support, VMware cannot continue to provide an operating model that matches the leading industry practices.

The high end of the datacenter space generally garners a lot of industry buzz, but as server average selling prices (ASPs) rise, volumes are constrained. The next megatrend is decentralization of compute from the large cloud datacenter. With 5G edge technologies focusing on latency-sensitive applications, there will be a need to build a larger set of edge computing devices to meet next-gen service requirements. Many services are sized on some simple metric such as one core per user, and so getting more cores to the network edge is important. Arm’s portfolio of Neoverse cores as well as Arm’s customer projects are building this higher core count and lower-power core for the edge.

Arm already dominates the edge, being found in mobile devices, but is also behind a large number of IoT devices as traditionally mechanical-only devices such as water pumps become networked. Sensors are increasingly being added to devices and data streamed back to the cloud. As these devices proliferate, the data they generate becomes too costly from an IT and environmental perspective to bring back to large centralized datacenters. Instead, data must be sifted through and important insights brought back to the cloud when further investigation is needed. This work drives demand for Arm not just at the edge device and the datacenter but also in the infrastructure that connects the two extremes.

For VMware, the implication is straightforward: it must either bring ESXi and its suite of products to these 5G edge deployments or it will cede this enormous market to the Linux hypervisor, Kubernetes, and public cloud deployments. Strategically, VMware needs to support Arm as that will be a dominant processor architecture at the edge simply due to its lower cost per core.
