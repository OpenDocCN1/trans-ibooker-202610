Finally, it uses the warpAffine() function from the OpenCV-Python library along with the rotation matrix computed and the desired output size as parameters to the function to obtain the aligned output image.

# # There is more

Start with the following screenshot (obtained from https://www.needpix.com/photo/801551/mona-lisa-painting-leonardo-da-vinci-portrait-young-face-substitution) with the Mona Lisa face present twice in two different orientations and use the dlib library's face detector and shape predictor to detect the faces along with the facial landmarks:

![](bbox=[0.325, 0.355, 0.67, 0.555])

Now, use the facial landmarks extracted to estimate an affine transformation (using scikit-image) and then warp the left face on the right face. You should get a figure like the following screenshot if you plot the detected faces and the warped face, along with the facial landmarks extracted:

![](bbox=[0.145, 0.67, 0.855, 0.875])