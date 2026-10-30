# CHAPTER 8

# # Virtual Machines
## on ESXi

While many readers may be looking forward to finding out how to create virtual machines (VMs) – which we will cover in this chapter – this may be the most unexciting chapter in this book, but for good reason: VMware has spent considerable time and effort to make creating VMs as easy as installing an OS on a physical machine. Yes, there are few nuances that you need to be aware of, and you will need to make sure that you get the correct ISOs for Arm processors, but in general the process of creating a VM on ESXi is straightforward.

In this chapter, I will provide you with step-by-step instructions for how to create a VM from a few different Linux ISO distributions. I will also discuss why Windows will not work with your system, how to make copies of your VMs, and what a snapshot is. Finally, I will give an overview of VMware Tools: what it is, why it is important, and how to install it on your Linux systems.

> **Note**  The ESXi-Arm-Fling-Doc states that Ubuntu, CentOS, openSUSE, Photon, Debian, and Fedora Linux distributions are known to be compatible; however, you need to make sure you get Arm versions of these distributions.