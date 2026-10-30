In a VDI environment, it is common to have up to five virtual CPUs for every physical CPU.

# ESXi on Arm on Pi Resource Usage Test

To check the resource usage of ESXi running on my Pi, I used ControlUp (Figure 2-26) and then the vSphere Client (Figure 2-27) without any running VMs. I found that it was using 18% of the RAM (~1.35GB) and virtually no CPU cycles, leaving 6.6GB RAM for the VMs running on it.

![](bbox=[0.092, 0.347, 0.867, 0.476])

**Figure 2-26.** *ESXi Resource Usage Test – ControlUp*

![](bbox=[0.092, 0.545, 0.867, 0.721])

**Figure 2-27.** *Resource Usage Test – vSphere Client*