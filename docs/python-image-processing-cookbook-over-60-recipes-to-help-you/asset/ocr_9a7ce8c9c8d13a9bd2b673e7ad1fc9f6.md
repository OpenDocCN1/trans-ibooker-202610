# # How it works...

Note that, for an image, the x axis is the vertical (+ve downward) axis and the y axis is the horizontal (+ve left-to-right) axis.

With the affine_transform() function, the pixel value at location o in the output (transformed) image is determined from the pixel value in the input image at position np.dot(matrix, o) + offset. Hence, the matrix that needs to be provided as input to the function is actually the inverse transformation matrix.

In some of the cases, an additional matrix is used for translation, to bring the transformed image within the frame of visualization.

The preceding code snippets show how to implement different affine transformations such as reflection, scaling, rotation, and shear using the affine_transform() function. We need to provide the proper transformation matrix, M (shown in the preceding diagram) for each of these cases (homogeneous coordinates are used).

We can use the product of all of the matrices to perform a combination of all of the affine transformations at once (for instance, if you want transformation T1 followed by T2, you need to multiply the input image by the matrix T2.T1).

If all of the transformations are applied in sequence and the transformed images are plotted one by one, you will obtain an output like the following screenshot:

![](bbox=[0.12, 0.55, 0.88, 0.842])