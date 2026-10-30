First, be aware that this string is space sensitive. If you have extra whitespace around your semicolon or forward slashes, you will generate errors. The first part of the string is the friendly name of the external library (no file extension). After the semicolon, type in the word Component followed by the name of the compiled binary resource, which will be identical to the original XAML resource dictionary.

That wraps up the examination of WPF’s resource management system. You will make good use of these techniques for most (if not all) of your applications. Next up, let’s investigate the integrated animation API of Windows Presentation Foundation.

# Understanding WPF’s Animation Services

In addition to the graphical rendering services you examined in Chapter 26, WPF supplies a programming interface to support animation services. The term animation may bring to mind visions of spinning company logos, a sequence of rotating image resources (to provide the illusion of movement), text bouncing across the screen, or specific types of programs such as video games or multimedia applications.

While WPF’s animation APIs could certainly be used for such purposes, animation can be used any time you want to give an application additional flair. For example, you could build an animation for a button on a screen that magnifies slightly when the mouse cursor hovers within its boundaries (and shrinks back once the mouse cursor moves beyond the boundaries). Or you could animate a window so that it closes using a particular visual appearance, such as slowly fading into transparency. A more business application-centric use is to fade in error messages on an application screen to improve the user experience. In fact, WPF’s animation support can be used within any sort of application (a business application, multimedia programs, video games, etc.) whenever you want to provide a more engaging user experience.

As with many other aspects of WPF, the notion of building animations is nothing new. What is new is that, unlike other APIs you might have used in the past (including Windows Forms), developers are not required to author the necessary infrastructure by hand. Under WPF, there is no need to create the background threads or timers used to advance the animation sequence, define custom types to represent the animation, erase and redraw images, or bother with tedious mathematical calculations. Like other aspects of WPF, you can build an animation entirely using XAML, entirely using C# code, or using a combination of the two.

> Note Visual Studio has no support for authoring animations using GUI animation tools. If you author an animation with Visual Studio, you will do so by typing in the XAML directly. However, Blend for Visual Studio (the companion product that ships with Visual Studio 2019) does indeed have a built-in animation editor that can simplify your life a good deal.

# The Role of the Animation Class Types

To understand WPF’s animation support, you must begin by examining the animation classes within the System.Windows.Media.Animation namespace of PresentationCore.dll. Here, you will find more than 100 different class types that are named using the Animation token.

These classes can be placed into one of three broad categories. First, any class that follows the name convention DataTypeAnimation (ByteAnimation, ColorAnimation, DoubleAnimation, Int32Animation, etc.) allows you to work with linear interpolation animations. This enables you to change a value smoothly over time from a start value to a final value.