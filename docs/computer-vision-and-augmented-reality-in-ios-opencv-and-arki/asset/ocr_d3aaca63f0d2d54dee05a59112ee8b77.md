```
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
```

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