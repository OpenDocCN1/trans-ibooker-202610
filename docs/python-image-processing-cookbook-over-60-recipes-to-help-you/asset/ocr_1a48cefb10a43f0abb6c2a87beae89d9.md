Start with the keyboard binary image and get rid of all of the symbols to get the output image shown here (hint: use binary erosion by reconstruction with an appropriate structuring element):

![](bbox=[0.186, 0.181, 0.81, 0.71])

Compare morphological diameter closing with closing by applying them to binary images where the results obtained are different when these two filters are applied.

Apply `mahotas` functions to apply the morphological local minima (and maxima) filters to a binary image. How are regional minima (and maxima) different from local minima (maxima)? Demonstrate with a binary image where these two are different. You can apply morphological filters to grayscale images as well (refer to the first link in the following See also and Grayscale operations sections for a demonstration of such an implementation).