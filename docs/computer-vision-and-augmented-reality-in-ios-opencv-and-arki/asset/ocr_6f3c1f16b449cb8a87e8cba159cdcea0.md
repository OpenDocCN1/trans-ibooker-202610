We only need the Y frame, which is held in the first place, as previously mentioned.

To do so, we use the `CVPixelBufferGetXOfPlane` techniques with a plane index of 0 instead of the standard `CVPixelBufferGet` methods.

Eventually, since the frame we obtain is in landscape mode, all we have to do is rotate it before moving it on to any OpenCV magic we have.

The logic is being reused.

If you’re making an Augmented Reality app that also supports iOS 10, you can use the preceding approach by changing the format of your AVCaptureVideoDataOutput to fit ARKit

```
_captureOutputVideo.videoSettings = @{
    (NSString *)kCVPixelBufferPixelFormatTypeKey :
    @(kCVPixelFormatType_420YpCbCr8BiPlanarVideoRange)};
```

and using the SampleBufferRef to get the same CVPixelBufferRef.

```
CVImageBufferRef imageBuffer =
CMSampleBufferGetImageBuffer(sampleBuffer);
```

# To Sum Up

Although iOS 11 (and beyond) install base has been increasing, a typical augmented reality application can not solely integrate the tools included in Apple’s latest update. However, by integrating the frame data we get from ARKit with OpenCV, we'll be able to build seemingly convincing interactions because they’re truly augmenting truth using both low-level implementation from OpenCV and ARKit’s convenience methods and APIs. Interactions can be anything from body tracking to depth analysis (Figure 7-1).