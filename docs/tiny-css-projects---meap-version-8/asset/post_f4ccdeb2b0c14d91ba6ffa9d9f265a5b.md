
#A Loader with added viewport of 100% width by 300 pixel height
#B The progress bar we will address later in the chapter

We have some starting CSS as well (Listing 3.2), the background, section, header, and paragraph have been pre styled in order to focus on the loader, progress bar, and animations.

## Listing 3.2 Starting CSS

```
body { background: rgb(0 28 47); }

section {
  display: flex;              #A
  flex-direction: column;     #B
  justify-content: space-between; #B
  align-items: center;        #B
  max-width: 800px;           #B
  margin: 40px auto;          #C
  font: 300 100% 'Roboto', sans-serif; #D
  text-align: center;         #D
  color: rgb(255 255 255);    #E
}                             #F

h1 {
  font-size: 4.5vw;
  margin: 40px 0 12px;
}

p {
  font-size: 2.8vw;
  margin-top: 0;
}
```

#A Start of rule styling the loader's container
#B layout
#C Margin written using the shorthand property. Top and bottom: 40px margin, left and right: auto.
#D Typography
#E Color
#F End of rule styling the loader's container

We can see that our section has its width capped at 800 pixels. <section> is a box-level element, therefore by default, it will take up the full width available to it. <body> and <html> are also box-level elements.

Since we don't specify a width, padding, or margin on either <body> or <html>, they will take the full width of the window. <section>, will take the full width of the <body>. However we assigned a maximum width to the <section>, therefore once the window reaches a width of 800 pixels or wider, the section will stop growing with the <body>, and remain at 800 pixels in width. Because it has a top and bottom margin of auto, it will also horizontally center itself relative to the <body>.

Our loader is contained within the section, the section will take the full width of the body up until it reaches 800 pixels therefore our loader will do the same. Figure 3.5 diagrams how the width of the loader will be affected by the screen size.

![](img/b56145aa9b751926a7aedbd7172bc818_70_0.png)

# Figure 3.5 Window width effect on SVG width

With the viewport set, let’s set the viewbox so that the contents of the SVG can scale with its container. Because remember, right now, we have only dealt with the frame, not the innards.

### 3.1.3 ViewBox

The viewbox sets the position, height, and width of the graphic within the viewport. Earlier we compared the viewport to a picture frame, the viewbox allows us to adjust the image to fit our frame. It can position the image, and also scale the graphic so that it can fit inside of the frame. We can think of the viewbox as our pan and zoom tools. To set the viewbox, we apply the viewBox attribute to the SVG with the following 4 values and syntax: viewBox="min-x min-y width height". Listing 3.3 shows the viewBox applied to our loader.

Dissecting the numbers in order, we start with min-x and min-y which are both set to 0. We want the top left corner of the graphic to be in the top left of our frame. Min-x and min-y allow us to adjust the position of the graphic in its frame; it’s the pan tool. Since we want it exactly in the top left corner, we set the values to 0.

Next we apply the width. It is set to 710 because our loader has 11 total bars each composed with a width of 60. 60 multiplied by 11 is equal to 660, we also have 10 gaps. The gap width between each bar is equal to, 5 × 10 = 50 therefore our loader’s width will be 660 + 50 = 710.

We will also base the height of the viewBox on the height of the bars in our loader. They will have a height value of 300, we therefore also set the viewport height to 300. Our loader will therefore fit exactly inside of its viewport.

## Listing 3.3 Declaring the viewbox

```
<svg viewBox="0 0 710 300" width="100%" height="300px">
<!--SVG code-->
</svg>
```

Notice that both our viewbox and viewport heights are equal to 300. This is how we zoom. If the viewbox figures are less than the viewport then you are effectively zooming out to the frame, the graphic will be smaller. If the viewbox figures are more than the viewport then you are zooming in. Since we have equal viewport and viewbox heights, we are not zooming.

Now that we have defined the space we will be working in, we can start adding shapes to the loader.

### 3.1.4 Shapes in SVG

There are a few standard SVG shapes/elements:

- `rect` (Rectangle )
- `circle`
- `ellipse`
- `line`
- `polyline`
- `polygon`

If we want to create an irregular shape, we can also use path. But we will not need it for this loader. Most often, they are what we see when we look at the XML behind logos, icons and complex animation graphics. For our project, we will use the basic rectangle shape to create the wave.

To define our rectangles which will create the bars in our loader, we will use the `<rect>` element and add 4 properties: `height`, `width`, `x`, and `y`. The x and y attributes will determine the position of the top left corner of the rectangle relative to the top left hand corner of the SVG.

We will want to create 11 rectangles (seen in Listing 3.4) that have a width of 60, a height of 300 and then we will use the x attribute to move the rectangles across the graphic. We will start at 0 and increase the value by the width of our bar (60) plus an additional gap of 5. Each rectangle’s x value will be 65 more than the previous. Our 11th rectangle should have an x value of 650.

```
Listing 3.4 11 rectangles
<svg viewBox="0 0 710 300" width="100%" height="300">
  <rect width="60" height="300" x="0" />
  <rect width="60" height="300" x="65" />
  <rect width="60" height="300" x="130" />
  <rect width="60" height="300" x="195" />
  <rect width="60" height="300" x="260" />
  <rect width="60" height="300" x="325" />
  <rect width="60" height="300" x="390" />
  <rect width="60" height="300" x="455" />
  <rect width="60" height="300" x="520" />
  <rect width="60" height="300" x="585" />
  <rect width="60" height="300" x="650" />
</svg>
```

We now have our rectangles positioned inside of our viewport and they are being correctly resized as we increase and decrease the window size by our viewBox. Figure 3.6 shows our

SVG at different window sizes. (A white border has been added to the SVG and bars in order to make them more visible in the screenshots). The contents shrink and grow within their available space, without skewing the rectangles they contain as the width to height ratio changes with the resizing of the window.

![](img/b56145aa9b751926a7aedbd7172bc818_72_0.png)

![](img/b56145aa9b751926a7aedbd7172bc818_72_1.png)

![](img/b56145aa9b751926a7aedbd7172bc818_72_2.png)

Figure 3.6 Added 11 rectangles inside of an SVG

Notice that our rectangles are black however, our next order of business will be to style them.

## 3.2 Applying styles to SVGs

We can apply styles to SVG elements in much the same way we do to HTML, inline, internally in a <style> tag, or in a separate stylesheet. However there are some minor differences.

First and foremost, how the SVG is being imported into our HTML affects where the styles need to live in order to affect the elements.

The easiest way to add a vector graphic to a web page is via an image tag. We reference the image file the same way we would any other image: `<img src="myImage.svg" alt="">`. We can also add them as a `background-image` inside of our CSS: `background-image: url("myImage.svg");`

In both of these cases our HTML and styles can affect the SVG itself but not the elements within. We can affect the size of the image for example but we cannot change the color of a particular shape contained inside the SVG. The image is essentially a black box we cannot penetrate to affect changes. In order to manipulate elements within it, the styles would need to be placed inside of the SVG itself.

Our third option, and the one we are using in this chapter is to place the SVG’s XML inline, directly in our HTML rather than in an external file preventing the black box issue we would encounter if the code was in an external file. The drawback is that our concerns are not as well separated because our image code is now mixed in with our HTML.

With our SVG placed inline in our HTML, the standard ways we apply CSS to any other HTML element apply. We can therefore place the styles we want to apply to our SVG inside of our CSS as it was any other HTML element.

## SVG Presentation Attributes

Unlike HTML where when applying styles inline we need to include a style attribute (ex: `<p style="background: blue">`), SVGs have a number of styles that can be added directly to the element as attributes. These are called **Presentation Attributes**. **fill** for example, (the SVG equivalent to `background-color`) can be applied directly to the element without a style tag: `<rect fill="blue">`.

These properties do not have to be applied inline directly on the element. They can be added inside of a style tag or stylesheet the same way we apply any other CSS style: `rect { fill: blue; }`.

A comprehensive list of SVG Presentation Attributes can be found at https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute/Presentation

Although the techniques to apply styles to our SVG elements remain the same as if they were HTML (except for the aforementioned SVG presentation attributes when applied inline) some of the properties we will use to style our elements will be different. Let’s take a closer look at the one we will be using for this project.

### 3.2.1 Fill

To set the background color of the loader bars, instead of using `background-color`, we will use the `fill` property, as the `background-color` property does not work for SVG elements. The fill property still supports the same values as the background-color such as color name, RGB(a), HSL(a) and hex. So instead of `rect { background-color: blue; }` we would instead write `rect { fill: blue; }`.

If no `fill` value is assigned to a particular shape, the `fill` will default to black, which is why our rectangles are currently black.
