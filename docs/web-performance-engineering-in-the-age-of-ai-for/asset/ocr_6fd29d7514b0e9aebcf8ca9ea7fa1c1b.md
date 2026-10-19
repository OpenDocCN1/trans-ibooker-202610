Chrome’s architecture means that the network runs in a separate process (the browser process or a dedicated network service process), so *network events* (like responses arriving) are delivered to the renderer asynchronously. The renderer’s main thread has to coordinate, parse, and execute network data, like HTML and XML HTTP requests (XHRs).

It’s also important to understand *incremental rendering*: browsers try to display content as soon as possible. They don’t wait for everything to be done. This means a page might paint some content (perhaps a header and spinner) after receiving a bit of HTML and CSS, even while the rest of the page is still loading. The largest contentful paint (LCP) metric often corresponds to one of these early renderings (ideally the first meaningful one).

# The Rendering Pipeline

In a renderer process, the pipeline for loading a page goes roughly as shown in Figure 5-1.

![](bbox=[0.14, 0.5, 0.86, 0.86])

Figure 5-1. The Chrome rendering pipeline, from HTML to pixels: navigation/resource fetching, parsing, styling, layout, painting, and compositing