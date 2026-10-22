![](bbox=[0.18, 0.13, 0.88, 0.26])

Figure 7.8 Floated compass with curved text

# 7.3.2 Clip-Path

This works because our image has a transparent background. We’ve curved the text, but the image is still quite square. If we add a background to the image, this becomes obvious. To make the image appear truly circular we need to add a clip-path (https://developer.mozilla.org/en-US/docs/Web/CSS/clip-path). Clip-path also takes a shape so we are going to pass it the same value we passed to shape-outside. We are also going to add some margin to our image to add a little bit of breathing room between it and the text. Listing 7.7 shows the complete css for our image.

# Listing 7.7 clip-path

```
img.compass {
  aspect-ratio: 1;
  float: right;
  shape-outside: circle(50%);
  clip-path: circle(50%);
  margin-left: 1rem;
}
```

We added a clip-path that matches our shape outside and a some margin to the left of the image to prevent the text from getting too close to the image especially since the compass has arrows protruding from the circular outline not accounted for by our circle. Figure 7.9 shows the finished output for the compass image.