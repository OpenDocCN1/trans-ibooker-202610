# # How it works...

The `euler()` function from the `mahotas` library was used to compute the Euler number (or characteristics) of the input binary image. By default, it uses 8-connectivity.

The `flood_fill()` function from the `skimage.morphology` module was used to perform flood filling on the black hole binary image, starting at a specific seed point (200,400) on the dark region inside the black hole (passed as the second argument to the function); the connected points equal to the `seed pixel value` (here, 0 or black) are found and then set to `new_value` (here, 1 or white), which is the third input parameter of the function.

The `diameter_closing()` function from the `skimage.morphology` module was used to remove all `holes` of the black hole image with `maximum extension` (a side of the `bounding box` enclosing a `hole`) smaller than the `diameter_threshold` parameter (that is, 100). This operator is also known as **bounding box closing**.

The `reconstruction()` function from the `skimage.morphology` module was used to implement dilation by reconstruction.

The dilation by reconstruction operation uses a `seed` and a `mask` image. The seed image specifies the values to start with (that `spreads` iteratively using the SE for `dilation`), whereas the mask image (the original image was used as the mask image) specifies the maximum value allowed at each pixel. The mask image puts a constraint on the `spread` of the `foreground` values.

The `BinaryMedianImageFilter()` constructor was used to instantiate the binary median filter (for denoising the supernova image) and the radius of the disk SE for this morphological filter was set using the `SetRadius()` method. The filter is applied to the input binary image using the `Execute()` method, and likewise for other SimpleITK filters.