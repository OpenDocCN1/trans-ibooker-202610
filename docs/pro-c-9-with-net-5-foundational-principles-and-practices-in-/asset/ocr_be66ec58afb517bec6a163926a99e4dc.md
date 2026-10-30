# Working with Vector Images

As you might agree, it would be quite challenging for a graphic artist to create a complex vector-based image using the tools and techniques provided by Visual Studio. Graphic artists have their own set of tools that can produce amazing vector graphics. Neither Visual Studio nor its companion Expression Blend for Visual Studio has that type of design power. Before you can import vector images into WPF application, they must be converted into Path expressions. At that point, you can program against the generated object model using Visual Studio.

> Note You can find the image being used (LaserSign.svg) as well as the exported path (LaserSign.xaml) data in the Chapter 26 folder of the download files. The image is originally from Wikipedia, located at https://en.wikipedia.org/wiki/Hazard_symbol.

# Converting a Sample Vector Graphic File into XAML

Before you can import complex graphical data (such as vector graphics) into a WPF application, you need to convert the graphics into path data. As an example of how to do this, start with a sample .svg image file, such as the laser sign referenced in the preceding note. Then download and install an open source tool called Inkscape (located at www.inkscape.org). Using Inkscape, open the LaserSign.svg file from the chapter download. You might be prompted to upgrade the format. Fill in the selections as shown in Figure 26-12.

![](bbox=[0.07, 0.5, 0.89, 0.68])

Figure 26-12. Upgrading the SVG file to the latest format in Inkscape

The next steps will seem a bit odd at first, but once you get over the oddity, it is a simple way to convert vector images to the correct XAML. When you have the image the way you want it, select the File ► Print menu option. Next, select the Microsoft XPS Document Writer as the printer target and then click Print. On the next screen, enter a filename and select where the file should be saved; then click Save. Now you have a complete *.xps (or *.oxps) file.