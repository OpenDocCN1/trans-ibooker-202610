+   4. Apply the hysteresis thresholding to the gradient magnitude (multiplied by zero-crossings) to find and link the edges. The lowerThreshold and upperThreshold parameters to the function are the hysteresis thresholds. Sure edges are the ones that have an intensity gradient value higher than the upperThreshold parameter. Sure non-edges have an intensity gradient value below the lowerThreshold parameter. The edges for which the intensity gradient values fall in between the hysteresis thresholds are classified as edges or non-edges, depending on whether or not they are connected to the sure-edge pixels.

The scipy ndimage module's gaussian_laplace() function (which accepts σ as a parameter for the Gaussian smoothing) was used to apply LoG convolution to the input image.

The algorithm to compute zero-crossing (implemented with the zero_crossing() function) is as follows:

+   1. Create a binary image from the LoG-convolved image: replace the positive and negative pixel values with ones and zeros, respectively.
+   2. Consider the direct neighbors of the non-zero pixels in the binary image obtained to compute the zero-crossing pixels.
+   3. Mark the boundaries by finding any non-zero pixel having an immediate zero neighbor.
+   4. Summarizing, for each non-zero pixel in the binary image, check whether any of its (eight) neighboring pixels is zero and, if so, identify the (center) pixel as an edge pixel—this is implemented using the any_neighbor_zero() function.

The pywt library's dwt2() function was used to implement DWT. The wavelet object passed as a parameter is a HAAR family wavelet.

The threshold_otsu() function from the scikit-image.filters module was used to binarize the edge (details) images obtained using DWT.