```
);
}
```

In the preceding method, we call the snapshotChanges method to get the data of the photos collection. We have already learned that a collection consists of doc objects. Thus, we can reach the actual photo object using the data method on the doc property for each action object.

+   6. Create the following method for adding all the photos to the viewer:

```
addPhotos() {
  const pinBuilder = new PinBuilder();

  this.getPhotos().subscribe(photos => {
    photos.forEach(photo => {
      const entity = {
        position: Cartesian3.fromDegrees(photo.lng, photo.lat),
        billboard: {
          image: pinBuilder.fromColor(Color.fromCssColorString('#de6b45'), 48).toDataURL()
        },
        description: `<img width="100%" style="margin:auto; display: block;" src="${photo.url}" />`
      };
      this.viewer.entities.add(entity);
    });
  });
}
```

The location of each photo on the viewer will be displayed as a pin. Thus, we need to initialize a PinBuilder object first. The preceding method subscribes to the getPhotos method to get all photos from Cloud Firestore. For each photo, it creates an entity object that contains the position, which is the location of the photo in degrees, and a billboard property that displays a pin of 48 pixels in size. It also defines a description property that will display the actual image of the photo when we click on the pin.