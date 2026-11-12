# See also

Refer to the following articles to learn more about this recipe:

+   - https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=4310039
+   - https://engineering.purdue.edu/kak/computervision/ECE661.08/OTSU_paper.pdf
+   - https://scikit-image.org/docs/dev/api/skimage.filters.html#skimage.filters.try_all_threshold
+   - https://mahotas.readthedocs.io/en/latest/thresholding.html
+   - Chapter 8, Image Segmentation, from the Packt Publishing book Hands-On Image Processing with Python.

# Image segmentation with self-organizing maps

A self-organizing map (SOM) is a competitive learning network (an interesting class of unsupervised machine learning), and it is one of the most popular neural network models. In this network, only one neuron gets activated at a given time, so the output neurons compete among themselves to be activated. This activated neuron is called the **winning neuron**. When one neuron fires, its closest neighbors tend to get more excited than ones that are further away (defining a topological neighborhood with decaying distance). As a result, the neurons are forced to organize themselves (through an adaptive or learning process) and a feature map between inputs and outputs is created. That's why this network is known as a self-organizing map.

The adaptive process of the SOM algorithm takes place in the following two steps:

+   1. **Ordering (self-organizing) phase**: The topological ordering of the weight vectors happens in this phase. Typically, this takes around 1,000 iterations.
+   2. **Convergence phase**: In this phase, the feature map is fine-tuned and forms an accurate statistical quantification of the input space. Typically, it takes at least as many iterations as 500 times the number of neurons.