You should see the rectangle shapes moving to the right, as seen in the following screenshot:

![](bbox=[0.1, 0.11, 0.92, 0.44])

Animation position

Let's jump back to the Main comp and preview our animation. Press the spacebar to preview our work:

+   1. In the Main comp, let's import a 1920 x 1080 image to use for our background in the lower third project
+   2. Drag the background image into the Main comp
+   3. Bring this image to the bottom layer in the timeline
+   4. Re-scale the image, if necessary

# Parenting null animations

Another animation technique that I want to introduce you to is called parenting animation. Through the process of parenting, any layer you parent to another layer will follow the movement or animation of that layer. In the process of parenting, you will have a parent and a child. A child is anything that is connected to, or parented by, the parent. The child is able to move freely. However, it will always be connected and thus move with the parent. Using a parenting animation, we can create a complex secondary animation. We are going to parent our two shape layers to a Null object. The Null object is a layer that is essentially invisible. However, this Null object can be animated and things can be parented to it.