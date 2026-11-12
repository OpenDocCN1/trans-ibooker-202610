# How it works...

We used SimpleITK's MorphologicalWatershedFromMarkers class for watershed segmentation implementation with morphological operators.

Watershed pixels were labeled 0. Output image labels were reordered in a way that the object labels became consecutive. Then those objects were sorted with respect to the object size with a RelabelComponent image filter.

# There's more...

Use connected-component labeling to segment the MRI image (obtained from https://github.com/loli/medpy/tree/master/notebooks/scripts/resources). You should get a figure like the following one:

![](bbox=[0.425, 0.405, 0.575, 0.52])

Implement region-growing segmentation with SimpleITK to segment an MR T1 image like the following figure:

![](bbox=[0.355, 0.598, 0.645, 0.865])