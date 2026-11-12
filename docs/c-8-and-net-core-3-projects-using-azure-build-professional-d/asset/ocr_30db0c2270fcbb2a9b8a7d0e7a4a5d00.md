Let's start with MainActivity.cs in EmotionDetector.Android. The OnRequestPermissionsResult method should look like this:

```
public override void OnRequestPermissionsResult(int requestCode, string[]
permissions, Android.Content.PM.Permission[] grantResults)
{
    Plugin.Permissions.PermissionsImplementation.Current.OnRequestPermissionsRe
sult(
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
[assembly: UsesFeature("android.hardware.camera.autofocus", Required =
false)]
```

If this hasn't automatically been added, then add it now.

Note that we have slightly diverted from the instructions here, in that we have declared that the camera is required. In most applications, the functionality would still work fine without the camera, but in this case, we really do need to use it.

![](bbox=[0.135, 0.745, 0.215, 0.805])

You should also consider whether your app can offer some degraded functionality, even if the user is unwilling to give access to requested resources. As we've mentioned previously, in this particular case, there's not too much we can do without the camera, but if you extended this application, maybe you would add the ability to upload existing pictures to the application – in which case, the app would still be usable without the camera.