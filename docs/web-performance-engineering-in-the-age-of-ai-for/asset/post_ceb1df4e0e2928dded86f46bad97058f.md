
up front, or failing to prioritize critical CSS and JavaScript.

## INP issues

AI-generated code doing heavy work on the main thread can make the app unresponsive. For instance, a common issue is an AI writing a slow JavaScript loop or a big JSON parse that blocks the thread, so when the user clicks a button, nothing happens for a second. Ensuring snappy interactivity often means breaking up long tasks, using web workers for heavy computations, or deferring noncritical work—strategies that typically require human intervention to implement.

## CLS issues

AI-generated code often overlooks the nuances that prevent layout shifts. For example, an AI might insert an <img> without specifying its width and height or including a placeholder space, meaning that when the image loads, it suddenly pushes text around. Or it might fetch data and inject a list of items above existing content, causing everything to reflow.

When reviewing AI-generated apps, you should immediately ask: How will this affect LCP? Could it hurt interactivity with any heavy computations or large synchronous tasks? Will it introduce layout shifts due to dynamic Document Object Model (DOM) changes without precautions? By keeping these questions in mind, you can systematically find and fix issues.

As you’ve seen from Chapter 2, modern tooling like Google Lighthouse can simulate a page load and give you LCP, CLS, INP, and other scores, often flagging specific problem areas. Running a Lighthouse audit on your AI-generated page is a great starting point to see where it stands. In essence, the CWVs act as a quantitative lens through which to view the quality of AI-generated code. Next, I’ll discuss typical problem patterns that cause poor scores and how to address them.

document.querySelector in an effect): such code can bypass React’s virtual DOM diffing and cause unexpected layout changes (most AI-generated React code sticks to React paradigms, but layout shifts can still occur). The key is to reserve space for content and avoid sudden DOM injections that shift existing elements.

## Bloated Bundles and Slow Loading (Poor LCP)

Another common pitfall is that AI-generated code might inadvertently create a large JavaScript bundle or load excessive resources, hurting the LCP. For instance, if you ask an AI to implement a certain UI component, it might import a heavy library for a small functionality. I’ve seen cases where an AI, in an attempt to create a date picker or chart, pulled in an entire library like Moment.js or D3 because it “knew” that would be one way to solve the problem. The result was adding hundreds of kilobytes of JavaScript, increasing download and parse times and delaying the rendering of the largest element. An experienced developer might have solved the same problem with a lighter approach or by code splitting the heavy component to load only when needed. An AI likely won’t do that unless prompted.

Another example is not deferring noncritical work. An AI might put a data fetch or an expensive computation right in a component’s initialization or top-level effect, without considering delaying it. If that code executes immediately on page load, it could delay the rendering of the main content. Also, AI might not add performance optimizations like <link rel="preload"> for critical assets or async/defer on noncritical scripts in an HTML document it generates, which a skilled performance engineer would use to improve load times.

To combat slow loads in AI-generated apps, profile the network and bundle. Use tools like Webpack bundle analyzers to see if the AI included something unexpectedly large. Check the Network panel in Chrome DevTools to observe how many requests are being made and their sizes. It’s not uncommon to find that an AI solution is making multiple sequential API

calls where one batched request would do, or loading an image at full resolution where a thumbnail would suffice. By catching these, you can refactor. You might split the code, using React’s lazy() and <Suspense> to load heavy components or libraries only when needed; remove or replace heavy libraries with lighter alternatives; and compress or resize images appropriately.

Running a Lighthouse audit will also call out issues like enormous network payloads or unused JavaScript, which often directly point to AI-induced bloat. If your AI-generated page has a poor LCP, those audits will give clues —perhaps a large script is blocking rendering or a big image wasn’t optimized.

Modern build tools and module bundlers (Webpack, Rollup, Parcel, etc.) allow us to split our JavaScript into smaller chunks. Instead of one monolithic bundle that includes everything (which increases initial download and parse cost), you can create multiple bundles and load some of them on demand. The simplest example is route-based splitting: for a single-page application, the code for each route (page) can be split and only loaded when the user navigates to that route. If your home page and dashboard have entirely separate code, why burden the home page with dashboard code that may never be used in that session?

Code splitting can drastically reduce the amount of JavaScript needed at initial page load. As a rule of thumb, performance experts often suggest that if your bundle exceeds ~50–100 KB (compressed), it’s worth splitting. With HTTP/2 and HTTP/3, the overhead of multiple small files is reduced (thanks to multiplexing and header compression), so the old fear of “too many files” is less acute. The emphasis is on loading “just enough” code to render what the user needs first. The team that runs Chrome’s V8 JavaScript engine puts it plainly: “Avoid having just a single large bundle; if a bundle exceeds ~50–100 kB, split it up into separate smaller bundles.” On mobile, the threshold is even lower due to slower networks and parse times.

How do you implement code splitting? You do it practically, often by using dynamic import() in JavaScript or lazy loading facilities in frameworks.

suppose you ask for a component that filters a list of items based on user input, and the AI integrates that logic:

```
function FilteredList({ items, query }) {
  const filteredItems = items.filter((item) => matchesQuery(item, query));
  // ... then maps filteredItems to JSX ...
}
```

This looks straightforward, but if items is large and this component rerenders frequently (say query is bound to a text input where the user types), it could become a performance nightmare. With every keystroke, the entire list (maybe thousands of items) is filtered on the main thread. This could cause noticeable input lag or even freeze if the list is big enough. A human developer might immediately think to debounce the filtering or move heavy computation to a web worker, but the AI-provided code won’t do that unless explicitly told. Similarly, an AI might sort data in the render path, perform deep copies of large objects, or update state in a loop—all patterns that can lead to long tasks that block the UI.

The result of such patterns is often a high total blocking time (TBT) in lab tests and poor interaction to next paint (INP) in the field. Users experience this as the UI stuttering or being unresponsive to clicks and keypresses. In extreme cases, the browser might even throw a “Page Unresponsive” warning if a JavaScript function runs for too long uninterrupted. Watch out for any AI-generated code that performs heavy synchronous work on the main thread.

Profiling the runtime performance is key here. Use the Chrome DevTools Performance panel to record a timeline while you interact with your page. The flame chart will show you if there are long tasks, like a 300-ms or 500-ms JavaScript function blocking the main thread. If you see a big chunk of activity corresponding to an AI-generated function, that’s a red flag.

So how do we address this issue? Optimize by splitting that work, using techniques like requestAnimationFrame to chunk it across multiple frames, or offloading it, perhaps using web workers for heavy computations
