

## 目录

- Business Continuity — 252
- Manageability — 253
- Data Locality — 253
- Types of Virtual Desktop Users — 256
- Accessing Virtual Desktops — 257
- Media Redirection — 258
- VDI Clients — 260
- Native Clients — 261
- Web Clients — 262
- All-in-One Client — 262
- Mobile Devices — 263
- What a Remote Protocol Does — 264
- Connecting to Virtual Desktops — 264
- Linux VMs — 265
- Fedora — 265
- Ubuntu — 266
- Connecting to Fedora Desktop with RDP — 269
- Installing RDP Server on Fedora — 270
- Connecting to a Linux Desktop Using RDP — 270
- X11 — 273
- Connecting to Fedora Using X11 — 274
- NoMachine (NX) — 280
- Connecting to a Virtual Desktop Using NoMachine — 281
- Changing Your Screen Resolution — 283
- Summary — 284

## Chapter 12: ESXi Networking — 285

Networking — 285

Virtual Switch — 287

Creating a Virtual Switch — 289

Physical NICs — 295

Virtual NICs — 296

Testing Performance of a Network — 297

iPerf3 — 298

Using iPerf3 VM-to-VM on the Same ESXi Host — 298

Using iPerf3 Between VM and Physical Machine — 302

Using iPerf3 Using the VMXNET3 Driver — 303

iPerf3 on ESXi — 307

Summary — 309

## Chapter 13: Managing ESXi Systems with VCSA — 311

vCenter Server License — 312

Installing vCenter Server — 313

Installing vCenter Server on an x64 ESXi Host — 316

Installing vCenter Server on VMware Workstation — 318

Configuring vCenter Server — 324

Adding ESXi Hosts to vCenter Server — 326

Managing ESXi Using vCenter Server — 331

Licensing — 331

Storage — 333

Networking — 336

Virtual Machines — 338

Cloning Virtual Machines — 339

vMotion — 341

- Storage vMotion — 343
- Automatic VM Startup with a Host — 344
- Using Third-Party Products with VCSA — 347
- ControlUp — 348
- Summary — 352
- **Chapter 14: The Future of ESXi on Arm** — 355
- Key Challenges for the Future of ESXi on Arm — 359
- How to Get Involved — 362
- Final Words — 366
- **Index** — 369

## About the Technical Reviewer

**Darren Hirons** is Lead Solutions Engineer at VMware. He has over 20 years of experience working with a wide variety of VMware products and other datacenter technologies. His focus more recently has been in the end user computing space, where he helps customers architect solutions to address business challenges.

## Acknowledgments

We freely acknowledge that the work and information in this book has been greatly shaped by the VMware ESXi on Arm community whose legions are far too numerous to mention. Their work and dedication to this project can be found in the many forums that they contribute to. A special shout-out needs to go to William Lam, a Senior Staff Solution Architect at VMware, for documenting so much about VMware technologies and, in particular, ESXi on Arm on his website https://williamlam.com/. William’s tireless work with the community is simply amazing!

## Preamble

All of the author’s proceeds from this book will go to Mount Hood Kiwanis Camp ([https://mhkc.org/](https://mhkc.org/)). This scenic camp sits on 22 acres of the US Forest Service land at the base of Mount Hood. It offers summer and winter camps that help empower children and adults with disabilities through overnight, outdoor, recreational programming. Both of us have had outstanding experiences with the Kiwanis organization over the years and gladly support this camp.

and performance reasons, each layer within this architecture could be composed of multiple servers delivering the services needed for the respective layer.

![](img/430161835ae267467687add4a1cdc429_19_0.png)

Figure 1-1. Three-Tier Architecture

During this time, a widely accepted best practice in datacenters was to have one physical server run a single service. The servers and operating systems (OSs) that ran on them didn’t have quality of service (QoS) features that would prevent an application from overconsuming resources on a server, which in turn prevented other active applications from running effectively.

Furthermore, we didn’t want any applications or OSs to bring down an entire multitiered application due to a software or hardware failure. Because it was impractical to “rightsize” the physical components for each physical server to match the load that was required of them by the services they were providing, this led to server sprawl (see Figure 1-2)—an overabundance of servers only using a small fraction of compute capacity that they were capable of.

![](img/430161835ae267467687add4a1cdc429_20_0.png)

Figure 1-2. Server Sprawl

As the demand for computer servers exploded, it became apparent to even the most casual observer that the current datacenter was unsustainable; a new technology and/or datacenter methodology was needed, and fortunately, a new technology was coming on the scene at this exact time. x86 operating system virtualization allowed multiple instances of an x86 operating system to run on a physical x86 server. These instances are now known as virtual machines, or VMs for short.

## Introduction to Virtual Machines (VMs)

VM technology is not new; in fact, IBM supported the use of VMs back in the mid-1960s on their mainframe computers, and, later on, Unix systems supported something akin to VMs. However, these VM technologies were not widely used in the datacenter for a variety of reasons, including complexity, cost, and the fact that they were only viable on expensive proprietary hardware.

## Type 2 Hypervisor

In the 2000s, a few companies devised hypervisor technology, a solution to run VMs on inexpensive commodity x86 servers. A hypervisor is the software or hardware that creates and runs VMs. In the early days of this revelation, we first had Type 2, or hosted, hypervisors that run on top of a base OS. Type 2 hypervisors can be basically thought of as just another application that is running VMs. In fact, you could run other applications, such as Microsoft Word, Excel, or even Solitaire, on the same base OS that is running the VMs (see Figure 1-3).

![](img/430161835ae267467687add4a1cdc429_21_0.png)

Figure 1-3. Type 2 Hypervisor

