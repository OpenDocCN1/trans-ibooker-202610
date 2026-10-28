```
this.storage.ref(name).putString(dataUrl,
    'data_url');
const photoUrl = await upload.ref.getDownloadURL();
\await
    this.firestore.collection<Photo>('photos').add({
        url: photoUrl,
        lat: latitude,
        lng: longitude
    });
}
```

First, we create a random name for our photo and use the putString method of the storage variable to upload it to Firebase storage. As soon as uploading has been completed, we get a downloadable URL using the getDownloadURL method, which can be used to access that photo. Finally, we use the add method to add a new Photo object in the collection property of the firestore variable. We use the collection property because we want to work with a list of photos in our application.

> Tip
> The firestore variable also contains a doc property that can be used when we want to work with single objects. The collection property internally keeps a list of doc objects.

10. Modify the takePhoto method to call the savePhoto method that we created in step 9:

```
async takePhoto() {
    const {latitude, longitude} = await
        this.getLocation();

    const cameraPhoto = await Camera.getPhoto({
        resultType: CameraResultType.DataUrl,
        source: CameraSource.Camera,
        quality: 100
    });
```