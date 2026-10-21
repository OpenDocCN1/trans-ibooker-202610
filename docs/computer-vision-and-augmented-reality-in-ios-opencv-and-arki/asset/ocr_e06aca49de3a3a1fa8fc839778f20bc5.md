The loop here will draw a circle around the eyes.

```
}
}

// Show Processed Image with detected faces
imshow( "Face Detection", img );
}
```

The function *imshow* will process the image if the face is detected as shown in the following output image:

![](bbox=[0.24, 0.33, 0.7, 0.62])

# B) Body Detection/Tracking

Tracking is the process of finding an object in a video frame by frame. Although the meaning seems direct, tracking is indeed an extremely broad concept within computer vision and machine learning, which includes conceptually related yet technically distinct concepts. For instance, object tracking encompasses many of the following distinct but interrelated concepts:

+   Thick optic flow: Such algorithms assist in calculating the motion vector within each pixel in a video frame.