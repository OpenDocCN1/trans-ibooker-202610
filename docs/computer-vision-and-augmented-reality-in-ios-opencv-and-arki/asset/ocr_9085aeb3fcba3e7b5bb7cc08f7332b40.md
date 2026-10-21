Here in this part of the code, the video is read, and you have to specify the path of the video file in VideoCapture.

```
57  // Read first frame
58  Mat frame;
59  bool ok = video.read(frame);
60
61  // Define initial bounding box
62  Rect2d bbox(287, 23, 86, 320);
63
64  // Uncomment the line below to select a different bounding box
65  // bbox = selectROI(frame, false);
66  // Display bounding box.
67  rectangle(frame, bbox, Scalar( 255, 0, 0 ), 2, 1 );
68
69  imshow("Tracking", frame);
70  tracker->init(frame, bbox);
71
72  while(video.read(frame))
73  {
74      // Start timer
75      double timer = (double)getTickCount();
76      // Update the tracking result
77      bool ok = tracker->update(frame, bbox);
78
79      // Calculate Frames per second (FPS)
80      float fps = getTickFrequency() / ((double)getTickCount() - timer);
81
82      if (ok)
83      {
84          // Tracking success : Draw the tracked object
85          rectangle(frame, bbox, Scalar( 255, 0, 0 ), 2, 1 );
86      }
87      else
88      {
89          // Tracking failure detected.
90          putText(frame, "Tracking failure detected", Point(100,80),
91  FONT_HERSHEY_SIMPLEX, 0.75, Scalar(0,0,255),2);
92      }
93
94      // Display tracker type on frame
95      putText(frame, trackerType + " Tracker", Point(100,20),
FONT_HERSHEY_SIMPLEX, 0.75, Scalar(50,170,50),2);
```

The video frames will be read here in the while loop. Then a bounding box will be created and then according to the tracker type it will be displayed on the video frame with the appropriate results.