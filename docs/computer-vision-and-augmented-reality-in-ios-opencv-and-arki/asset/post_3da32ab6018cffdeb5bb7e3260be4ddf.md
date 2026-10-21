
# OpenCV Tracking API

OpenCV 4 includes a tracking API with configurations of such a variety for individual object tracking algorithms. BOOSTING, MIL, KCF, TLD, MEDIANFLOW, GOTURN, MOSSE, and CSRT are eight trackers included within OpenCV 4.2. In this section we will elaborate further on the differences and the pros/cons of each configuration.

**To be considered:** Only the following six trackers are implemented in OpenCV 3.2: BOOSTING, MIL, TLD, MEDIANFLOW, MOSSE, and GOTURN. And the following five trackers are implemented in OpenCV 3.1: BOOSTING, MIL, KCF, TLD, and MEDIANFLOW. BOOSTING, MIL, TLD, and MEDIANFLOW are the only four trackers implemented in OpenCV 3.0.

**Update:** The tracking API in OpenCV 3.3 has improved.
When implementing the trackers, an OpenCV version check is recommended before calling the appropriate API configurations. Let’s have a look at the setup and usage as we implement the tracking algorithm. To begin implementing the tracker, we start by representing the tracker options in an array to allow us to simply switch between the tracker forms – BOOSTING, MIL, KCF, TLD, MEDIANFLOW, GOTURN, MOSSE, or CSRT – as shown in the following code with comments:

```
1 #include <opencv2/opencv.hpp>
2 #include <opencv2/tracking.hpp>
3 #include <opencv2/core/ocl.hpp>
4 
5 using namespace cv;
6 using namespace std;
7 
8 // Convert to string
9 #define SSTR( x ) static_cast< std::ostringstream & >( \
10 ( std::ostringstream() << std::dec << x ) ).str()
11 
12 int main(int argc, char **argv)
13 {
14     // List of tracker types in OpenCV 3.4.1
15     string trackerTypes[8] = {"BOOSTING", "MIL", "KCF", "TLD","MEDIANFLOW",
16     "GOTURN", "MOSSE", "CSRT"};
        // vector <string> trackerTypes(types, std::end(types));
```

At the start of the code, OpenCV and tracking libraries have to be included, and add a list of the tracker types in OpenCV 3.4.1.

```
// Create a tracker
string trackerType = trackerTypes[2];

Ptr<Tracker> tracker;

#if (CV_MINOR_VERSION < 3)
{
    tracker = Tracker::create(trackerType);
}
#else
{
    if (trackerType == "BOOSTING")
        tracker = TrackerBoosting::create();
    if (trackerType == "MIL")
        tracker = TrackerMIL::create();
    if (trackerType == "KCF")
        tracker = TrackerKCF::create();
    if (trackerType == "TLD")
        tracker = TrackerTLD::create();
    if (trackerType == "MEDIANFLOW")
        tracker = TrackerMedianFlow::create();
    if (trackerType == "GOTURN")
        tracker = TrackerGOTURN::create();
    if (trackerType == "MOSSE")
        tracker = TrackerMOSSE::create();
    if (trackerType == "CSRT")
        tracker = TrackerCSRT::create();
}
#endif
```

In this part, a tracker has been created, and all tracker types added previously will be used.

```
// Read video
VideoCapture video("videos/chaplin.mp4");

// Exit if video is not opened
if(!video.isOpened())
{
    cout << "Could not read video file" << endl;
    return 1;
}
```

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

```
97  // Display FPS on frame
98  putText(frame, "FPS : " + SSTR(int(fps)), Point(100,50),
99  FONT_HERSHEY_SIMPLEX, 0.75, Scalar(50,170,50), 2);
100 
101  // Display frame.
102  imshow("Tracking", frame);
103 
104  // Exit if ESC pressed.
105  int k = waitKey(1);
106  if(k == 27)
107  {
108      break;
109  }
110 }
111 }
```

Finally, the frame is displayed on the screen after the tracking is complete and the bounding box is created. The imshow function handles displaying the fully rendered frame in the user interface. And the if statement handles when the exit key is pressed to cancel the tracking.

We’ll look at a few various tracking algorithms throughout this portion. The aim is to gain real knowledge of each tracker, rather than a piece of deep theoretical knowledge.

To begin, I’ll go through some basic tracking concepts. Our aim of tracking will be to locate an object throughout a current frame after accurately tracking it in (or approximately all) prior frames. A motion model seems to be simply a nice way of saying that you know the object’s position and velocity (velocity + direction of movement) in subsequent frames. We can estimate a new position depending on the present motion model unless we are unable to detect or analyze the object; therefore it is essential to be relatively familiar what the object is to detect where it is in realtime. You may create an appearance model that describes how the object appears. To more accurately predict the position of the object, this appearance model could be utilized to search within a tiny neighborhood of the motion model’s predicted location. Positive (object) and negative (context) instances are fed to a classifier to train it. Let us just take a look at how different tracking algorithms solve the online training issue.

## BOOSTING Tracker

This tracker is based on an online version of AdaBoost, which is the internal algorithm of the Haar cascade-based face detector. This classifier must be conditioned for both positive and negative instances of the object during runtime. The classifier is run with each pixel in the vicinity of the previous position in a new frame, and the classifier’s score is registered. The object’s new position is the one with the highest score. As a result, we now have yet another positive example for the classifier. Since more frames are received, the classifier is modified with the new information.

Advantages: Its algorithm is over a decade old and still works, but I couldn’t think of a better purpose to do it while there are other specialized trackers (MIL, KCF), which operate along with similar principles.

Disadvantages: Output in terms of tracking is mediocre. It doesn’t always understand whenever tracking isn’t working.

## MIL Tracker

It is similar to the BOOSTING tracker. The major difference is that, rather than finding just the object’s current position as a positive example, this searches a small community surrounding the current location for many possible positive examples. You don’t specify positive and negative examples in MIL; instead, you specify positive and negative “packs.” The positive bag’s array of photographs isn’t entirely made up of positive examples. Instead, just one picture from the optimistic bag must be a good illustration! A positive pocket, in our example, contains a patch based on the object’s current position as well as patches from a small community around it.

Advantages: The result is excellent. It does not drift even more than the BOOSTING tracker, so this performs admirably when partially obscured. This might be the better tracker accessible unless you’re using OpenCV 3.0. However, if you’re using a later version, KCF is a good option.

Disadvantages: The error of tracking is not accurately recorded. After a complete occlusion, it does not heal.
