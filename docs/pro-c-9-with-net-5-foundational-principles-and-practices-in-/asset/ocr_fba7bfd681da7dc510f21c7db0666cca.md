```
{
    _currImage=0;
}
imageHolder.Source=_images[_currImage];
}
```

At this point, you can run your program and flip through each picture.

# Embedding Application Resources

If you would rather configure your image files to be compiled directly into your .NET Core assembly as binary resources, select the image files in Solution Explorer (in the \Images folder, not in the \bin\Debug\Images folder). Change the Build Action property to Resource and set the Copy to Output Directory property to Do not copy (see Figure 27-2).

![](bbox=[0.105, 0.352, 0.73, 0.645])

Figure 27-2. Configuring the images to be embedded resources

Now, using Visual Studio’s Build menu, select the Clean Solution option to wipe out the current contents of \bin\Debug\Images and then rebuild your project. Refresh Solution Explorer and observe the absence of data in your \bin\Debug\Images directory. With the current build options, your graphical data is no longer copied to the output folder and is now embedded within the assembly itself. This ensures that the resources exist but also increases the size of your compiled assembly.

You need to modify your code to load these images into your list by extracting them from the compiled assembly.

```
// Extract from the assembly and then load images
_images.Add(new BitmapImage(new Uri("@/Images/Deer.jpg", UriKind.Relative)));
_images.Add(new BitmapImage(new Uri("@/Images/Dogs.jpg", UriKind.Relative)));
_images.Add(new BitmapImage(new Uri("@/Images/Welcome.jpg", UriKind.Relative)));
```