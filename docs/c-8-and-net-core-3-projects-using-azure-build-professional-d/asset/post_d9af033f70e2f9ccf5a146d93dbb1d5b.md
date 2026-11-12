

## Xamarin plugins

The principle behind Xamarin is that the code that you write is cross-compiled. That is, when you compile for Android, you end up with a native app targeting the Android platform, and when you compile for iOS, you end up with a native app targeting iOS. However, the Xamarin platform is unable to do this when a piece of code is platform-specific. Say for example, I want to access the camera – this varies from platform to platform. You can’t simply take the code that does this for an iPhone and compile it for Android. So, how do we create platform-specific code?

The answer is that each piece of platform-specific code needs to be created separately for each platform; the principle behind Xamarin Forms is that you call the method in a polymorphic fashion. Let’s take the camera as an example (since that’s what we’re about to use); the code that takes a picture on Android is clearly different to that on iOS. When we install the plugin, each environment implements that plugin’s code in a platform-specific way. From our point of view, though, we simply install the plugin and call a few lines of code and it works everywhere.

## Media plugin

Installing the plugin itself is actually just as straightforward as adding a NuGet package:

```
Install-Package Xam.Plugin.Media -ProjectName EmotionDetector
```

> It’s worth bearing in mind that these plugins are constantly changing;
> while they are correct at the time of writing, you should follow
> the readme.txt file as a primary source.

Once you’ve done this, the plugin displays a readme.txt file with instructions on how to install it. Please follow those instructions as they are likely to change by the time you read this. I will detail the Android instructions here for completeness as some of the instructions may not be as verbose as they could be.

Let's start with MainActivity.cs in EmotionDetector.Android. The OnRequestPermissionsResult method should look like this:

```
public override void OnRequestPermissionsResult(int requestCode, string[]
permissions, Android.Content.PM.Permission[] grantResults)
{
    Plugin.Permissions.PermissionsImplementation.Current.OnRequestPermissionsResult(
        requestCode, permissions, grantResults);
}
```

Anything that you want to do in a mobile app these days has to be approved by the user; if you need access to the user's contacts, phone, or camera, then the user will be presented with a dialog indicating that your app needs these permissions. The code in this method is called after the user responds to that.

You will have to declare which permissions your app needs, and this should be automatically added by the plugin. If you have a look inside EmotionDetector.Android.Properties.AssemblyInfo.cs, you should see the following:

```
// Add some common permissions, these can be removed if not needed
[assembly: UsesPermission(Android.Manifest.Permission.Internet)]
[assembly:
UsesPermission(Android.Manifest.Permission.WriteExternalStorage)]

[assembly: UsesFeature("android.hardware.camera", Required = true)]
[assembly: UsesFeature("android.hardware.camera.autofocus", Required = false)]
```

If this hasn't automatically been added, then add it now.

Note that we have slightly diverted from the instructions here, in that we have declared that the camera is required. In most applications, the functionality would still work fine without the camera, but in this case, we really do need to use it.

![](img/762492a8287f369cc1ac4621a212f193_427_0.png)

You should also consider whether your app can offer some degraded functionality, even if the user is unwilling to give access to requested resources. As we've mentioned previously, in this particular case, there's not too much we can do without the camera, but if you extended this application, maybe you would add the ability to upload existing pictures to the application – in which case, the app would still be usable without the camera.

The AndroidManifest.xml file should look like this:

```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionCode="1" android:versionName="1.0" package="com.companyname"
    android:installLocation="auto">
    <uses-sdk android:minSdkVersion="21" android:targetSdkVersion="27" />
    <application android:label="EmotionDetector.Android">
        <provider android:name="android.support.v4.content.FileProvider"
            android:authorities="${applicationId}.fileprovider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_paths"></meta-data>
        </provider>
    </application>
</manifest>
```

This is used to support a subdependency of the media plugin. As you can see, it references a specific resource directory, and so the next step is to create that directory. In the Android Resources folder, create a new xml folder:

![](img/762492a8287f369cc1ac4621a212f193_428_0.png)

Create a file inside this folder labeled file_paths.xml and add the following code:

```
<?xml version="1.0" encoding="utf-8"?>
<paths xmlns:android="http://schemas.android.com/apk/res/android">
  <external-files-path name="my_images" path="Pictures" />
  <external-files-path name="my_movies" path="Movies" />
</paths>
```

Finally, the MainActivity.cs module needs an additional line in its OnCreate method:

```
protected override void OnCreate(Bundle savedInstanceState)
{
    TabLayoutResource = Resource.Layout.Tabbar;
    ToolbarResource = Resource.Layout.Toolbar;

    base.OnCreate(savedInstanceState);
    CrossCurrentActivity.Current.Init(this, savedInstanceState);

    Xamarin.Essentials.Platform.Init(this, savedInstanceState);
    global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
    LoadApplication(new App());
}
```

The CrossCurrentActivity plugin is another plugin in the same ecosystem. It allows the media plugin to access MainActivity.

### TakePicture()

Now that we've added the plugin, we can create the code so that we can use the camera. Let's have a quick look at this method:

```
private async Task<MediaFile> TakePicture()
{
    string fileName = $"FaceImg_{DateTime.Now.Ticks}";

    MediaFile photo = await
    Plugin.Media.CrossMedia.Current.TakePhotoAsync(new
    Plugin.Media.Abstractions.StoreCameraMediaOptions()
    {
        SaveToAlbum = true,
        Name = fileName
    });

    if (photo != null)
    {
        return photo;
    }
    return null;
}
```

There's not much to this method since the plugin does the bulk of the work for us. The parameters that we're passing in allow the image to be saved, and we're declaring a unique name for it.

> During testing, you may decide to switch off the save feature; otherwise, your phone will fill up with pictures of you pulling strange faces to try to look angry, sad, or happy!

If the photograph is successfully taken, we return a `MediaFile`; otherwise, we return `null`.

### GetEmotionAnalysis()

Our next method is responsible for calling the Azure API and getting a response. There is a client API for this, however. At the time of writing, it was still in preview, and seemed to have various issues. Should you wish to investigate using this instead, then a good place to start is the NuGet repository: https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.Face/.

However, we will be making use of only a part of this package, so let's install it now:

```
Install-Package Microsoft.Azure.CognitiveServices.Vision.Face -Version 2.4.0-preview
```

In this section, we will manually build up the call to the API; here's the code for our method:

```
private async Task<IList<DetectedFace>> GetEmotionAnalysis(Stream imageStream)
{
    var byteData = GetImageAsByteArray(imageStream);

    return await MakeAnalysisRequest(byteData,
        "https://uksouth.api.cognitive.microsoft.com/face/v1.0/detect",
        "a9ac2b7404fd45ed9aff787f158e24c7");
}
```

