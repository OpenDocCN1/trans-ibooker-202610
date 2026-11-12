When you call CaptureScreenshot, the capture doesn't happen immediately. Instead, Unity waits for the end of the frame when all rendering has completed; at that point, the screenshot is captured and saved to disk.

> **TIP**
> 
> This means that you can capture only one screenshot per frame. Each call to CaptureScreenshot is effectively a “request” to capture a screenshot later, and only one request will be stored at a time.

In addition to saving the screenshot as a file, you can save it as a texture by using the CaptureScreenshotAsTexture method:

```
// Capturing a screenshot as a texture.
// Note that this captures the screenshot immediately, and it may not
// contain what you expect.
Texture2D capturedTexture =
ScreenCapture.CaptureScreenshotAsTexture();
```

Unlike CaptureScreenshot, CaptureScreenshotAsTexture does capture the screenshot immediately, because it needs to return the captured texture. This means that the texture may not contain the final image that you expect.

To make it capture at the end of the frame, you need a coroutine that waits for the end of the frame before capturing the texture, such as in this example:

```
IEnumerator CaptureScreenshotAtEndOfFrame() {
    // Wait until the very end of the frame, after all rendering has
    // completed
    yield return new WaitForEndOfFrame();

    // Capture the screenshot
    Texture2D capturedTexture =
    ScreenCapture.CaptureScreenshotAsTexture();
}
```