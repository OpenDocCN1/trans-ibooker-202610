# Expanding a virtual disk

Hardware changes to virtual machines are quick and easy, due to the nature of VMs and using all virtual resources that can be expanded or condensed with simple clicks of the mouse. Increasing or decreasing CPU cores and RAM resources is very self-explanatory and happens from inside the VM Settings menu. One other specific task that is a very normal procedure, but is not quite so straightforward, is the expansion of a virtual hard disk upon which a VM is running.

You can see in the following screenshot that my WEB3 server has a D: volume, but some yahoo (me) only allocated 5 GB initially, and it's almost full.

![](bbox=[0.13, 0.27, 0.87, 0.4])

Figure 14.30: My data drive needs more room

On a physical server, this could be complicated, installing a newer and bigger drive and then needing to migrate everything to it. Since this is a VM, could we make things easier by adding a new disk that is larger and then copying everything over, saving us the step of dealing with hardware? Certainly. But there is a much simpler way even than that to deal with disks running out of space. Let's expand the existing data disk! The best part is that this only takes a few minutes:

Open Hyper-V Manager, and under Actions on the right side of the screen, select Edit Disk....

Run through the Edit Disk wizard, specifying the virtual hard disk file (VHDX file) that you want to manipulate, and on the third screen of the wizard, select the option to Expand this disk. Following the Expand selection, you will be presented with the current size of the disk, and a field where you can populate the new size. I'll go ahead and quadruple the size of this disk from 5 GB to 20 GB:

![](bbox=[0.13, 0.7, 0.87, 0.83])

Figure 14.31: Expanding my data disk on the fly