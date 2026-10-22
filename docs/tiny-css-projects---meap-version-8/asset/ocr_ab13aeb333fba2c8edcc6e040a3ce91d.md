![](bbox=[0.18, 0.12, 0.88, 0.31])

Figure 5.11 Background image clipped to the heading

# 5.4 Styling the cards

Each card is created using an outer section element which will have a background image, and an inner div which we will give a background color to in order to keep our text legible over the image. Within that div is the actual content. Listing 5.9 shows our card structure.

# Listing 5.9 Card HTML

```
<section> #A
  <div> #B
    <h2>Meeru Island Resort & Spa</h2>    #C
    <p>The stylish Meeru Island Resort...</p>    #C
    <a href="#">Learn more</a>    #C
  </div>
</section>
```

#A Outer Card container
#B Content container
#C Content

To style each part of the card, we will work outwards first, styling the container for each card, then the container for the content, and then finally the content itself.

# 5.4.1 Outer card container

The outer container is the element that will get the background image. Each section gets an image for its hotel or resort. We will select each section individually by individually selecting immediate descendent section elements of main in conjunction with the :nth-child() pseudo-class. Our selectors are built as follows: main > section:nth-child(n) where n is the position of the card we are trying to target within the main element. We then assign each of the sections a background image as seen in listing 5.10.