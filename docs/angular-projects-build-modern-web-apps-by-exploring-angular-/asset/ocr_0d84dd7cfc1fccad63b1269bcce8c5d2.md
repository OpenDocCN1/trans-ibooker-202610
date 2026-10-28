The camera button on the page needs to open the camera to take a photo. In the following section, we will learn how to use Capacitor to interact with the camera.

# Interacting with Capacitor

Taking photos in our application involves using two APIs from the Capacitor library. The **Camera** API will open the camera to take a photo, and the **Geolocation** API will read the current location from the GPS. Let's see how we can use both in our application:

+   1. Execute the following npm command to install both APIs:
+   
+      ```bash
+      npm install @capacitor/camera @capacitor/geolocation
+      ```
+   
+   2. Create an Angular service using the following Ionic CLI command:
+   
+      ```bash
+      ionic generate service photo
+      ```
+   
+   3. Open the `photo.service.ts` file and add the following import statements:
+   
+      ```typescript
+      import { Camera, CameraResultType, CameraSource } from '@capacitor/camera';
+      import { Geolocation } from '@capacitor/geolocation';
+      ```

+   4. Create a method in the PhotoService class to read the current position from the GPS device:
+   
+      ```typescript
+      private async getLocation() {
+        const location = await
+          Geolocation.getCurrentPosition();
+        return location.coords;
+      }
+      ```

The getCurrentPosition method of the Geolocation object contains a coords property with various location-based data such as the latitude and the longitude.

+   5. Create another method that calls the getLocation method and opens the camera of the device to take a photo:
+   
+      ```typescript
+      async takePhoto() {
+        await this.getLocation();
+   
+        await Camera.getPhoto({
+      ```