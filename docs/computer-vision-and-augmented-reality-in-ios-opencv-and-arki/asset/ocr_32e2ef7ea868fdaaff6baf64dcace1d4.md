```
protected virtual void OnDisable ()
{
    ARFaceComponent.updated -= TryToUpdateModelStatus;
    ARSession.stateChanged -= TryToUpdateModelStatus;
}

private void TryToUpdateModelStatus (ARFaceUpdatedEventArgs eventArgs)
{
    TryToUpdateModelStatus();
}

private void TryToUpdateModelStatus (ARSessionStateChangedEventArgs eventAr;
{
    TryToUpdateModelStatus();
}

private void TryToUpdateModelStatus ()
{
    bool isFaceVisible = GetFaceVisibility();
    ModelTransform.gameObject.SetActive(isFaceVisible);

    if (isFaceVisible == true)
    {
        ModelTransform.localPosition = ARFaceComponent.vertices[AR_(
    }
}

private bool GetFaceVisibility()
{
    return enabled == true && ARFaceComponent.trackingState != Tracking!
}
```

# ARFace Data and Face Detection in ARKit 3

The procedure is straightforward: when the ARFaceManager detects a face, it sends new ARFace data. Since our controller is linked to ARFace’s updated case, the script updates the location of the glasses whenever a new set of face point coordinates is detected.

The vertex array indexes often apply to the same characteristic points on a human’s face; the only thing that varies based on a person’s facial characteristics is the overall location and face mesh. We aligned our glasses to vertex index 16, yielding the following outcome.