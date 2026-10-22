# Listing 7.10 dog.svg

```
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 152 193">
  <defs>
    <style>
      .cls-1{
        fill:none;
        stroke:#000;
        stroke-miterlimit:10;
        stroke-width:2px;
      }
    </style>
  </defs>
  <path class="cls-1" d="M21.9135,115.62c-17.2115,4.7607-37.3354,..."/>
</svg>
```

We have defs, which includes the styles for the image. This part defines what the individual elements in the svg will look like. Then we have a path. The path is the element displaying the dog, however it is 1988 characters long, quite complex, and when pasted into the path function as follows shape-outside: path('M21.913...'); does not seem to do anything. This is because as of this book being written, no browser has path() fully implemented⁶.

Once this feature is implemented, creating our paths using a graphics editor and copying them over to create our shapes will be a very valuable technique, but one that is not available to us yet. This method also comes with the drawback that these paths can get quite long, which makes maintainability dubious. However, we have a couple of alternatives:

+   1. Creating a polygon shape that roughly matches our image, similarly to the technique we used above with the circle.
+   2. Using the url() function which pulls in the image and bases the shape on the alpha channel.

We are going to go with option 2 and use the url() function.

# 7.4.2 Using the image

Same as when we handled the compass image, we are going to start by floating the image, but to the left this time to break up the visual monotony in our page. Then, to create the shape we use the url() function to which we pass the path to the image. Listing 7.11 shows the css applied to the dog image.