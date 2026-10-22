grid to style our project, and by the end of the chapter, our layout will look as follows (Figure 2.1):

![](bbox=[0.18, 0.19, 0.9, 0.44])

Figure 2.1 Final output

Our starting HTML found in the chapter-02 folder of the Github repository (https://github.com/michaelgearon/Tiny-CSS-Projects) looks as follows:

# Listing 2.1 Project HTML

```
<body>
  <main> #A
    <header> #B
      <img src="./img/logo.jpg" alt="">
      <h1>Sonnets by William Shakespeare</h1>
    </header>
    <article> #B
      <h2>
        Sonnet 1
        <br><small>by William Shakespeare</small>
      </h2>
      <p>
        <span>From fairest creatures we desire increase,</span>
        ...
      </p>
    </article>
    <aside> #B
      <section aria-label="sonnet 2">
        <img src="./img/image-1.jpg" alt="">
        <h3>Sonnet 2</h3>
        <p>
          When forty winters shall besiege thy brow,
          <br>And dig deep trenches in thy beauty's field, ...
        </p>
      </section>
```