Listings 7.1 and 7.2 include our starting CSS and HTML for our page that we build upon in this chapter. To follow along as we style the page you can download the starting HTML and CSS from the github repo at: https://github.com/michaelgearon/Tiny-CSS-Projects/tree/main/chapter-07.

Our HTML consists of a main tag inside of which we have a header, blockquote, three paragraphs, 2 images, and the citation for where the text came from.

# Listing 7.1 Starting HTML

```
<main>
<h1>Chapter I: Into the Primitive</h1>
<blockquote>"Old longings nomadic..."</blockquote>
<p>Buck did not read the newspapers, or he...</p>
<img class="compass" src="../img/compass.png" #A
     width="175" alt="a black and gray compass"> #A
<p>Buck lived at a big house in the...</p>
<img class="dog" src="../img/dog.svg" #B
     width="126" alt="line drawing of a dog"> #B
<p>And over this great demesne Buck ruled...</p>
<cite>London, Jack...</cite>
</main>
```

#A The compass image
#B The Dog Image

Our CSS includes some base styles to set up our page including margin, padding, and background color. The body's width is restricted to be no wider than 78ch, and margins center the content when the screen width exceeds our maximum value. Also set up is the default font for the page which is Time new Roman. Lastly, in order to ensure the images do not overflow on small screens, they are given a maximum width, which is set to 100%. In other words, they can't be wider than their container.

# Listing 7.2 Starting CSS

```
html {
  padding: 0;
  margin: 0;
}

body {
  background-color: rgba(206, 194, 174, 0.24);
  padding: 4rem;
  font-size: 16px;
  max-width: 78ch; #A
  margin: 0 auto; #C
  font-family: 'Times New Roman', Times, serif;
  border-left: double 5px rgba(0,0,0,.16);
  min-height: 100vh; #B
  box-sizing: border-box;
}
img {
  max-width: 100%;
}
```