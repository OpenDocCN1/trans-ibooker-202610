# Browser-Native Lazy Loading for Iframes

Many third-party embeds, like YouTube videos and maps, are in iframes. The easiest way to lazy load these is to use the native loading="lazy" attribute on the <iframe> tag, much like you’d add loading="lazy" on an <img>. For example:

```
<iframe src="https://www.youtube.com/embed/VIDEO_ID"

width="560" height="315"

loading="lazy"

title="YouTube video player"

frameborder="0" allowfullscreen></iframe>
```

All modern browsers support iframe lazy loading via this attribute. The browser will hold off on loading the iframe’s content until it’s close to being in the viewport. The exact distance threshold varies by browser but is measured in screen heights. The nice thing is that the browser automatically handles this one line of code as a hint.

One important consideration when lazy loading iframes is to reserve space for the content. If you don’t specify its width and height (or CSS dimensions), when the iframe eventually loads, it might cause a layout shift, harming CLS, because the browser didn’t allocate space for it initially. Always set explicit size attributes or CSS for lazy-loaded iframes.

Browser-native lazy loading is completely passive—you don’t need JavaScript to handle it. This is great for simplicity and reliability. However, there are a few caveats:

+   - Some browsers load conservatively to avoid popping content in late. If an iframe is just below the fold, one browser might load it eagerly, while another might wait until the user scrolls.
+   - Older browsers that don’t support lazy loading will just ignore this code and load the iframe immediately, as usual. There’s no harm;