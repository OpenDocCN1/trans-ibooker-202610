# Corporate Sandboxes

Closely associated with home lab systems are corporate sandboxes. These systems tend to be used for small-scale proof-of-concept systems to prove out new applications and processes. Some companies use older systems for this purpose; however, their support cost can easily be more than the cost of a couple Pi systems.

Many corporations have licensing agreements with VMware that make the consumption of ESXi licenses within the facility very attractive. Given that a standard vCenter Server allows up to 2,000 hosts, adding a few additional ESXi hosts for a corporate sandbox usually isn’t an issue.

# Lightweight VDI Hosts

Virtual desktop infrastructure (VDI) is the process where users connect to VMs running a desktop OS on a hypervisor. VDI has been going strong for over a decade now, and many organizations have VDI environments with tens of thousands of virtual desktops. Companies use VDI for security, business continuity, and cost containment reasons.

While you will not be able to run thousands of virtual desktops on a Pi given its limited resources, I have been able to host four virtual desktops on an ESXi Pi server (Figure 3-2). This is an interesting proposition as you can host them all on a device that is slightly larger than a deck of cards. Using these desktops, I was only able to complete ordinary tasks using LibreOffice and light web browsing using a 1920 × 1080 desktop.