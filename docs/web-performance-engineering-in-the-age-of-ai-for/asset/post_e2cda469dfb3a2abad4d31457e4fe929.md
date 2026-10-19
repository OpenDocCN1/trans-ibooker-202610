
information it needs (like resource hints and dimensions) to do its job well.

For the curious, tools like Chrome’s trace viewer can show how tasks and requests were prioritized and executed: record a performance trace and look at the “Blink scheduler” and “Network” tracks. You might see a script task labeled “user-blocking” or another labeled “background” if you’re using the new API.

By mastering both high-level best practices and low-level mechanics, you’ll become a performance engineer—capable not only of implementing optimizations but also of debugging the thorniest performance issues and understanding *why* something helps or not.

[OceanofPDF.com](OceanofPDF.com)

## Chapter 6. Trade-Offs in Performance Optimizations

> *Web development is a series of trade-offs.*
> 
> — Tim Kadlec

Every performance enhancement comes with a cost: increasing complexity, adding maintenance, even potentially hurting some other metric. This chapter takes a pragmatic look at strategies for improving JavaScript performance—and the trade-offs they entail. The goal is to help you evaluate optimizations objectively, balancing explicit recommendations with context. As Tim Kadlec wisely points out, there is rarely a one-size-fits-all solution in web development; we operate in a “grey goo” of possibilities. You have to know your priorities (speed, accessibility, developer productivity, etc.) and choose your optimization strategies accordingly.

## Balancing Different Performance Metrics

As you’ve seen throughout this book, performance itself isn’t one metric—it’s many. Sometimes you need to trade one off for another. For example, to improve largest contentful paint (LCP), you might delay some script that is needed for interactivity, which could hurt interaction to next paint (INP) if the user tries to interact quickly. Or a strategy to optimize time to interactive (TTI) might increase the total load time slightly. You have to decide which metrics are more important for your scenario.

For a concrete example: if you have heavy JavaScript that doesn’t affect above-the-fold content, you might postpone it until after the first paint. This improves LCP because the image or text loads faster; it might also improve INP since it leaves the main thread free at the start. However, if the user tries to interact early, some functionality might not be ready. If that happens rarely, it’s fine; if it’s common, you might need to ensure that critical interaction handlers aren’t being delayed and perhaps load skeleton handlers that will show a spinner until the rest is ready.

It’s a game of priorities. Make sure that any trade-off still leaves the user with a *consistently good experience*. No one likes a site that loads really quickly but then is unresponsive when you tap. No one enjoys an interactive site that gives them nothing to look at for seconds at a time. So aim to get something onto the screen quickly (optimize the rendering path) *and* make sure there are no long blocks where user input is ignored (optimize main-thread scheduling).

## Objective Metrics Versus Subjective Perception

When making trade-offs, rely on data. Use real performance metrics: measure load times and interactivity times using Core Web Vitals like LCP and INP (see Chapter 4). If switching to a lighter framework or removing a library improves LCP by one second on 3G, that’s huge and worth maybe making a dev’s life slightly harder. If an optimization changes a lab test by five ms but adds three days of work each sprint to maintain, though, it’s probably not worth it.

Also consider the *distribution* of your users. If 90% of your audience is on high-end corporate laptops on fast Wi-Fi and just 10% are on mobile, you might set a different budget than if 90% are on midrange phones. Alex Russell’s p75 guideline suggests paying attention to devices at the 75th percentile and above, at the least. Depending on your product, you might explicitly target certain environments sometimes. Know who’s using your product and on what devices.

## Common Trade-Offs

In this section, I’ll discuss the trade-offs in decisions like bundling versus splitting and framework abstractions versus vanilla JavaScript, as well as when to invest effort in micro-optimizations. Then we’ll move on to how to set performance budgets.

## Bundle Size Versus Network Overheads

In Chapter 4, I recommended code splitting as a way to reduce the initial size of the JavaScript. That’s generally a good practice, but there is a trade-off: if you split your code into too many small chunks, that could incur additional HTTP requests or increase the total download bytes, due to bundle overhead or because you’re duplicating the runtime boilerplate in each chunk. HTTP/2 reduces the per-request cost but doesn’t eliminate it completely: for example, if each chunk has its own wrapper or needs its own fetch, 100 tiny chunks might incur more overhead than 10 moderately sized chunks.

So there’s a balance: you want to isolate code that can be delayed or conditionally loaded, but you don’t want to split so much that the initial page ends up pulling in hundreds of files, which can cause slowdowns for other reasons. It’s often helpful to analyze real user data or use tools like Webpack’s Bundle Analyzer to see how your code is split and if there’s duplication among chunks. Let’s look at some examples:

- Sometimes splitting can cause code duplication. For instance, if two lazy-loaded chunks share a common library, it might get included in both, increasing total bytes. The solution in that scenario might be to factor the common code into a separate, shared chunk that loads early or on demand.
- Suppose you have a large form on your page that uses a 50 KB validation library. You could either include it in the main bundle or load it only when the user navigates to the form and focuses on the first input. Including it up front adds 50 KB to everyone’s load—

## Frameworks Versus Vanilla JavaScript

A decision with one of the biggest trade-offs in web development is whether to use frameworks or heavy libraries to speed up development, or write more optimized, perhaps leaner code by hand. It’s ultimately a question of prioritizing the developer experience versus the user experience.

Frameworks like React, Angular, and Vue provide immense productivity and consistency benefits, but a very developer-friendly approach—like using lots of high-level abstractions or multiple analytics scripts for every need—might harm UX if it bloats the app.

However, as Tim Kadlec’s analysis shows, sites that use large frameworks often incur a significant performance penalty in terms of JavaScript bytes and CPU time. To quantify this, Kadlec pulled data from HTTP Archive. He found that median scripting time for React sites was about 9.3 s on mobile versus about 2.3 s for all sites. That’s a huge difference. Angular and Vue had similarly heavy costs, while jQuery sites were closer to average (jQuery is smaller and not a full SPA framework).

This doesn’t mean that “frameworks are bad” outright. It highlights a trade-off: those frameworks make it easier to build complex apps, but you’re trading initial performance for that convenience. You can mitigate their cost through techniques like tree-shaking or server-side rendering (SSR), but there’s always some overhead, since the framework’s own code and abstractions must run.

If raw performance was the only goal, you might write an app with vanilla JavaScript or a minimal library, tuned to exactly what you need. But that could take much longer to develop and be harder for a team to maintain. The trick is finding balance. Some options to consider:

Use frameworks, but be disciplined

For example, use a framework for the main UI but not for a simple landing page—maybe that page can be mostly static HTML. You could use a lighter-weight framework like Preact, Solid, or Svelte when possible to reduce runtime overhead. You could also trim parts of a framework you don’t use or augment it with performance
