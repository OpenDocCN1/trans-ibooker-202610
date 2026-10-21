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