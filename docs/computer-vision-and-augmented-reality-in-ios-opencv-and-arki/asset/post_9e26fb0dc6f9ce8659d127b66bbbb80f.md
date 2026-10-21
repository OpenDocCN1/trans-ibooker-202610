
// Change path before execution
cascade.load( "../../haarcascade_frontalcatface.xml" );

// Start Video..1) 0 for WebCam 2) "Path to Video" for a Local Video
capture.open(0);
if( capture.isOpened() )
{
    // Capture frames from video and detect faces
    cout << "Face Detection Started...." << endl;
    while(1)
    {
        capture >> frame;
        if( frame.empty() )
            break;
        Mat frame1 = frame.clone();
        detectAndDraw( frame1, cascade, nestedCascade, scale );
        char c = (char)waitKey(10);

        // Press q to exit from window
        if( c == 27 || c == 'q' || c == 'Q' )
            break;
    }
    else
        cout<<"Could not Open Camera";
    return 0;
}

Use 0 to initiate the web cam stream. After that, face detection will start.

```
void detectAndDraw( Mat& img, CascadeClassifier& cascade,
                    CascadeClassifier& nestedCascade,
                    double scale)
{
    vector<Rect> faces, faces2;
    Mat gray, smallImg;

    cvtColor( img, gray, COLOR_BGR2GRAY ); // Convert to Gray Scale
    double fx = 1 / scale;

    // Resize the Grayscale Image
    resize( gray, smallImg, Size(), fx, fx, INTER_LINEAR );
    equalizeHist( smallImg, smallImg );

    // Detect faces of different sizes using cascade classifier
    cascade.detectMultiScale( smallImg, faces, 1.1,
        2, 0|CASCADE_SCALE_IMAGE, Size(30, 30) );
```

This part of the code detects different sizes of faces using a cascade classifier.

```
for ( size_t i = 0; i < faces.size(); i++ )
{
    Rect r = faces[i];
    Mat smallImgROI;
    vector<Rect> nestedObjects;
    Point center;
    Scalar color = Scalar(255, 0, 0); // Color for Drawing tool
    int radius;

    double aspect_ratio = (double)r.width/r.height;
    if( 0.75 < aspect_ratio && aspect_ratio < 1.3 )
    {
        center.x = cvRound((r.x + r.width*0.5)*scale);
        center.y = cvRound((r.y + r.height*0.5)*scale);
        radius = cvRound((r.width + r.height)*0.25*scale);
        circle( img, center, radius, color, 3, 8, 0 );
    }
    else
        rectangle( img, cvPoint(cvRound(r.x*scale), cvRound(r.y*scale)),
            cvPoint(cvRound((r.x + r.width-1)*scale),
            cvRound((r.y + r.height-1)*scale)), color, 3, 8, 0 );
    if( nestedCascade.empty() )
        continue;
    smallImgROI = smallImg( r );
```

This chunk of code draws a circle around the face by detecting the shape and uses a scalar color as a drawing tool.

```
nestedCascade.detectMultiScale( smallImgROI, nestedObjects, 1.1, 2, 0|CASCADE_SCALE_IMAGE, Size(30, 30) );
```

This detects the eyes from the input image.

```
for ( size_t j = 0; j < nestedObjects.size(); j++ )
{
    Rect nr = nestedObjects[j];
    center.x = cvRound((r.x + nr.x + nr.width*0.5)*scale);
    center.y = cvRound((r.y + nr.y + nr.height*0.5)*scale);
    radius = cvRound((nr.width + nr.height)*0.25*scale);
    circle( img, center, radius, color, 3, 8, 0 );
}
```

The loop here will draw a circle around the eyes.

```
}
}

// Show Processed Image with detected faces
imshow( "Face Detection", img );
}
```

The function *imshow* will process the image if the face is detected as shown in the following output image:

![](img/7d8e54ee4578088e252f4c75b5cd4c88_82_0.png)

## B) Body Detection/Tracking

Tracking is the process of finding an object in a video frame by frame. Although the meaning seems direct, tracking is indeed an extremely broad concept within computer vision and machine learning, which includes conceptually related yet technically distinct concepts. For instance, object tracking encompasses many of the following distinct but interrelated concepts:

- Thick optic flow: Such algorithms assist in calculating the motion vector within each pixel in a video frame.

Sparse optical flow: The Kanade-Lucas-Tomasi (KLT) feature tracking is one of these algorithms that track the position from some feature points in a picture.

Kalman filtering: A common signal processing algorithm is utilized prior motion knowledge to predict the position of an object moving. The guidance of missiles was one of the first implementations of this algorithm! “The on-board computer which led the descent of the Apollo 11 lunar module to the moon had a Kalman filter,” according to the article.

Meanshift and Camshift: The maxima of a density function can be found using these algorithms. They’re also employed in the tracking process.

Single-object tracker: The first frame is labeled with a rectangle to show the position of the object we would like to track within this class of trackers. The tracking algorithm is then used to track the object in subsequent frames. Such trackers are typically utilized in combination with an object detector in real-world applications.

Finding multiple object tracking algorithms: When using a quick object detector, it’s a good idea to detect several objects from every frame and use a tracking algorithm to figure out whether the rectangle within each frame correlates to a rectangle in the next.

Detection vs. tracking: If you’ve used OpenCV face detection, you find how this operates within real time or could immediately spot a face in each frame. What’s the big deal about tracking within that initial position? Let us just look at why you would need to track objects in a clip rather than only doing repetitive detections.

Tracking is quicker as compared to detection: Tracking algorithms are usually quicker as compared with detection algorithms. An explanation is straightforward. While tracking an object that has been observed throughout the previous frame, we have a lot of information regarding that object’s presence. We now understand the former frame’s location, as well as the moving location and velocity. Thus, within the coming frame, we will utilize all this knowledge for an estimate of where an object will be

within the coming frame or also conduct a tiny search across the predicted location to precisely place an object. An effective tracking algorithm can use all of the knowledge it knows regarding an object through the level, whereas a detection algorithm would always begin with a scratch. As a result, when constructing such an effective approach, the object detection algorithm being typical runs every nth frame, with the tracking algorithm used in the n-1 frames (n minus 1, where n is the total number of frames in the video). You can just track the object after detecting this within the first frame. Although tracking has advantages when we have additional data (i.e. a large set of images or a video), but it could still be dropped and missed if an object goes underneath a barrier over some longer length of time or moves too quickly for the tracking algorithm to keep it going. It’s also normal for tracking algorithms to acquire bugs, as a bounding box that’s tracking an object gradually moves away through this. A detection algorithm has been applied so often to solve these problems with tracking algorithms. A huge example of its object is used to train detection algorithms. As a result, they are very knowledgeable regarding an object’s specific class. Tracking algorithms, across the opposite side, are most knowledgeable regarding a particular example of a class we have been tracking.

When detecting stalls, tracking will assist: When a facial detector is used in a clip but an object obscures a person’s face, it would quite probably fail. A strong tracking algorithm, across either side, could deal with deformation to a certain extent.

Identification is preserved by tracking: Object detection produces a series of rectangles containing an object as an output. The item, on the other hand, has no identification. A detector that detects red dots, for example, can emit rectangles referring to every dot that has been detected in a frame as in the clip here. This would produce a new series of rectangles in the first frame. The specific dot could be portrayed with a rectangle positioned at index 10 throughout the array within a first frame, as well as at index 17 in the second frame. We have no indication which rectangle refers to which item when utilizing detection onto a single frame.
