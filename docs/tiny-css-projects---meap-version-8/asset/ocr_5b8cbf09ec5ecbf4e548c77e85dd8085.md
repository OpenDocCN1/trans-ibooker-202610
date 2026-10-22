We’ll be creating the rectangles using an SVG graphic and take a look at what SVGs offer us to understand the slight differences that exist in CSS properties between styling HTML elements and SVG elements.

For the progress bar, we will use the HTML `progress` element and then look at how we can remove the browser’s default styles and apply our own. Overall we want to create a consistent, responsive loader that can work across devices. The end result we will achieve is shown in figure 3.1.

The code for the before and after for this project can be found in the GitHub repository: https://github.com/michaelgearon/Tiny-CSS-Projects

![](bbox=[0.185, 0.31, 0.9, 0.675])

Figure 3.1 End goal for this chapter

# 3.1 SVG and graphics

SVG stands for Scalable Vector Graphics. SVGs are written using an XML-based markup language and consists of vectors on a Cartesian plane.

A vector is a mathematical formula that defines a geometric primitive. Lines, polygons, curves, circles, and rectangles are all examples of geometric primitives.

A Cartesian coordinate system in a plane is a grid based system that defines a point using a pair of numerical coordinates based on its distance from 2 perpendicular axes. Where these two axes cross is the origin, which has a coordinate value of (0, 0). Think back to math class, when we were being asked to plot lines on a graph, that was a cartesian coordinate system. Essentially, SVGs are shapes on a coordinate plane and are written in XML.