# A typical trajectory using single-observation SGD

![](bbox=[0.19, 0.19, 0.88, 0.4])

Figure 8.10 Illustrating the typical trajectory of single-observation SGD (on the left) and the corresponding loss curve (on the right). Both contain two regions: the far-out zone, where the current iterate is far away from the loss optimum, and the region of confusion, where the current iterate is in the vicinity of the optimum.

This behavior is further accompanied by the right panel, where the loss drops quickly at the beginning within the far-out zone and then suffers from random oscillations after entering the region of confusion.

Note that the SGD updates with a minimal learning rate at each step could still produce a stable trajectory, as shown in figure 8.11. Sensitivity to the learning rate reflects only when it is large.