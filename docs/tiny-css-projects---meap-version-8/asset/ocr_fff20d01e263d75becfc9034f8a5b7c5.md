# 4.2.4 Putting it all together

With our counter-style defined, we can now apply it to our list. Remember that we named the counter-style rule “emoji”; we will apply the name as the list-style property value for our list as seen in listing 4.8.

# Listing 4.8 Styling the bloquote

```
@counter-style emoji {  #A
  symbols: "\2615";  #B
  system: cyclic;
  suffix: " ";
}

article ul {
  list-style: emoji;    #C
}
```

#A at-rule defining the custom list-style's behavior
#B ☕
#C Applying the custom list-style to the article's lists

Figure 4.10 shows our newly styled list.

![](bbox=[0.18, 0.43, 0.9, 0.66])

Figure 4.10 List styled using ☕ as counters

Let’s continue going down the page and style the image next.

# 4.3 Styling images

Historically, newspapers were printed in black and white, colored ink in newsprint is a fairly new thing when we consider the history of print. To give our design a bit of a retro vibe, we will therefore make our image grayscale. We will first look at how to alter our image using filters. Unlike print, on the web we need to worry about resources not loading or links being