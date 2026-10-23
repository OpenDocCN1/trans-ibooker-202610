
## Discussion

Unity supports a wide variety of file formats:

*3D objects*
Autodesk FBX and Collada; Unity can also import Maya, Cinema 4D, 3ds Max, Cheetah3D, Modo, LightWave, Blender, and SketchUp files, if the corresponding software is installed.

*Audio*
WAV, MP3, OGG, and AIFF; Unity also supports a variety of tracker module formats—specifically, Impulse Tracker (.it), Scream Tracker (.s3m), Extended Module File Format (.xm), and Module File Format (.mod).

*2D textures*
Adobe Photoshop, BMP, PNG, JPG, BMP, and TGA.

*Text*
TXT and JSON.

![](img/e54c00769db669e3746fa1e50090241f_33_0.png)

Unity will pick up most changes made to files in external programs, but often relies on that specific program being installed. For example, if you want to work with Blender .blend files as assets in Unity, then you'll need to have the Blender application installed on the same system.

## 1.7 Building Unity Projects

### Problem

You want to configure Unity to build your game, so that you can distribute the game to players.

### Solution

To build your game, open the Build Settings view by opening the File menu and choosing Build Settings (Figure 1-13).

![](img/e54c00769db669e3746fa1e50090241f_34_0.png)

Figure 1-13. The Build Settings view

When you build your game, you specify which scenes should be included. If you haven't already saved your current scene, you should do so now by opening the File menu and choosing Save, or by pressing Command-S (Control-S on a PC). You can drag and drop the scenes you want to include into the Scenes In Build list, or you can click the Add Open Scenes button to add all scenes that you have open right now.

Next, you need to select which platform you want to build for. Unity supports a wide range of platforms, ranging from desktop PCs to mobile phones to consoles and more. Only one platform can be selected at a time; the current platform will be marked with a Unity logo next to it. To build for a different platform, select it and click the Switch Platform button.

When you’re ready, click the Build button. Unity will ask you where you want to save the build; once that’s done, Unity will start building.

## Discussion

To build for certain platforms, you need to download the appropriate platform support module. If you don’t have the necessary module, you won’t be able to build; to get the module, click the platform and click the Open Download Page button. You’ll be taken to the appropriate web page for downloading the module.

![](img/e54c00769db669e3746fa1e50090241f_35_0.png)

Certain platforms, like consoles, require a special license; see the Unity Platform Module Installation page for more information.

## 1.8 Accessing Preferences

### Problem

You want to access the settings that control Unity’s behavior, both for the entire application and for your current project.

### Solution

To open the per-project settings, open the Edit menu, and choose Project Settings. The Project Settings view will appear (Figure 1-14).

![](img/e54c00769db669e3746fa1e50090241f_36_0.png)

Figure 1-14. The Project Settings view

In the Project Settings view you can configure a wide range of possible settings for your game, including the maximum resolution, build name, graphical quality settings, input, and more.

For settings that affect the entire Unity application itself, access the Preferences view.

## Discussion

The Preferences view (Figure 1-15) allows you to configure things like colors, hotkeys, and other settings that apply to all projects. Access these settings on a PC by opening the Edit menu and choosing Preferences; on a Mac, open the Unity menu and choose Preferences.

Figure 1-15. The Preferences view

## 1.9 Installing Unity Packages

### Problem

You want to install a Unity package, to add extra features to the Unity engine.

### Solution

You can use the Unity Package Manager to install new packages (which typically add new features).

Packages live inside a specific project. So if you install a package, you’ve installed it for the project you’re working with at the time, and would need to install it again for other projects.

To find the Package Manager, open the Window menu and select Package Manager. You’ll be able to search for and install new packages (which are downloaded from the internet).

## Discussion

Unity’s Package Manager allows various components to be distributed separately, and updated separately from the engine.

![](img/e54c00769db669e3746fa1e50090241f_38_0.png)

You may see the Unity Package Manager referred to as “UPM.”

You’ll work with a few packages in this book, and most features of Unity are distributed as packages these days. You can learn more in the Unity documentation.

## CHAPTER 2

## Scripting

Creating behaviors in Unity means writing code, or scripting. Scripts in Unity are written in C# (pronounced “C sharp”). C# was originally developed by Microsoft, and has since evolved to include a full, fantastic open source implementation, which Unity makes extensive use of.

![](img/e54c00769db669e3746fa1e50090241f_39_0.png)

We strongly recommend that you review Chapter 1 before you work through any of the recipes in this chapter!

This book isn’t going to try to give you a complete guide to programming, or a complete guide to programming in C#—that’d be a whole other book. Instead, we’ll show you how to write scripts for use in Unity, using C# as the language.

![](img/e54c00769db669e3746fa1e50090241f_39_1.png)

There are other languages you can use to write code in Unity. For example, there are a number of bindings to other languages, and Unity supports a range of visual scripting tools that we’ll touch on in other chapters, but that are beyond the scope of this book.

If you’re interested in learning C# in a broader sense, we strongly recommend C# 7.0 in a Nutshell by Ben and Joseph Albahari (O’Reilly), as well as Head First C# by Andrew Stellman and Jennifer Greene (O’Reilly).

## 2.1 Adding a Script to an Object in a Unity Scene

### Problem

You want to add scripts to your game objects, so that you can give them behavior using C# code.

### Solution

Unity game objects are containers for *components*. A MonoBehaviour—named for *Mono*, the scripting runtime that Unity uses—is a script component. When a Mono Behaviour is attached to a game object, it participates in the game’s update loop and gives you the opportunity to run code every frame.

![](img/e54c00769db669e3746fa1e50090241f_40_0.png)

Even if you’re building something other than a game, such as a simulation, the basic Unity object is currently called a game object. So we’re going to stick to that terminology.

To add code to a game object, you create a new MonoBehaviour script and attach it to the game object as a component. This means that you’ll need a game object to use. In a Unity scene:

- 1. Create a new cube by opening the GameObject menu and choosing 3D Object → Cube.
- 2. Use the Move tool to position the cube to somewhere in front of the camera.

Next, we’ll create a new script:

- 1. Go to the Project tab and right-click the Assets folder. Choose Create → C# Script.
- 2. A new file will be created, and Unity will prompt you to name it. Name the file “Mover.”

![](img/e54c00769db669e3746fa1e50090241f_40_1.png)

Unity uses the name you provide to determine the contents of the file. It’s important that the C# classes that you write have the same name as the file they’re in, since Unity uses that information to save the contents of the scene. If you rename the file, be sure to rename the class that’s inside it as well.

3. Double-click the script. Unity will open it in your editor: by default, Visual Studio if you’re on a PC, Visual Studio for Mac if you’re on a Mac, or your text editor if you’re on Linux.

![](img/e54c00769db669e3746fa1e50090241f_41_0.png)

You can customize which program Unity uses to edit scripts by going to Unity’s preferences and choosing External Tools → External Script Editor. We’re particularly keen on Visual Studio Code and BBEdit on macOS.

You’ll see code that looks like this:

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Mover : MonoBehaviour
{
    void Start()
    {
    }

    void Update()
    {
    }
}
```

![](img/e54c00769db669e3746fa1e50090241f_41_1.png)

Start is called before the first frame update, and Update is called once per frame.

4. Modify the Mover class to look like this:

```
public class Mover : MonoBehaviour
{
    public Vector3 direction = Vector3.up;

    float speed = 0.1f;

    void Update()
    {
        var movement = direction * speed;
```
