
The actual website

Tracking scripts, tracking pixels, remarketing, etc.

Figure 8-1. WebPageTest's waterfall view can highlight the impact of heavy third-party script use

## Measuring Third-Party Scripts’ Performance Impact

Before you can optimize third-party scripts, you need to identify them and quantify their impact on your site’s performance. This involves using performance auditing tools and techniques to see what third-party code is present and how it affects loading metrics. You’re already familiar with tools like Chrome DevTools, Lighthouse, and WPT from Chapter 2. Here, I’ll focus on applying them specifically to third-party script analysis, highlighting what to look for and how to interpret the results.

## Inventory and Identification: Key Metrics and Indicators

The first step is to inventory all third-party scripts on a given page. This includes both scripts added directly into your HTML (like a `<script src="https://third-party.com/script.js">` tag) and those dynamically injected by other scripts or tag managers.

The key is to establish a baseline: know what third-party scripts are on the page, where they come from, and some measure of their cost in bytes, load time, execution time, and so on. When evaluating third-party scripts, metrics to pay attention to include:

- How many HTTP requests are coming from third-party domains? (Generally, more requests mean more overhead.)
- How much data is being transferred from third parties, in total KB/MB? This affects load on slow networks. If third parties dominate the bytes, that’s a red flag.
- How many milliseconds on the main thread (CPU) are spent executing third-party JavaScript? This directly affects TTI and responsiveness.
- Do third-party requests start after your main content, or are they interfering by starting too early? Ideally, critical content should

- Be particularly cautious with AI-generated code that integrates third-party services. As noted in Chapter 4, AI tools don’t inherently optimize for performance. If you’re using AI to generate integration code for third-party services, you must manually review it for performance best practices: check if the code uses async loading, uses proper error handling, and doesn’t block critical rendering paths.

When reviewing any third-party code (whether AI-generated or manually written), ensure it follows the optimization patterns discussed in later sections of this chapter.

## Lighthouse and Chrome DevTools

In Chrome DevTools, the Network panel is a great starting point: you can load your page and filter and group requests by domain. Any requests to domains that aren’t yours (or your CDN’s) are third-party, so they’re easy to spot.

Tools like PageSpeed Insights and Lighthouse (which is integrated into Chrome DevTools) offer automatic analyses that list third-party resources. For example, Lighthouse has an audit called “Reduce the impact of third-party code,” which enumerates third-party scripts, showing their sizes and main-thread execution times. It’s an easy way to get a quick list of all external script providers and how costly each one is. Other useful Lighthouse audits include:

- “Reduce JavaScript execution time” (previously “JavaScript boot-up time”)
- “Avoid enormous network payloads”
- “Eliminate render-blocking resources”
- “Reduce unused JavaScript”

Another way to identify third-party script execution in Chrome DevTools’ Performance panel is to record a performance trace while loading the page,

then group your profiling data by script origin. To do this, go to the “Bottom-Up” or “Call Tree” view, group by “Product.” Chrome will label groups like “Google Analytics” or “Facebook SDK” if it recognizes them. This tells you how much CPU time was spent executing script from each domain. Sorting by total time quickly shows which third-party scripts are consuming the most CPU during page load.

The Coverage tab in DevTools’ Sources panel shows how *much* of each script was executed. This can sometimes reveal that you loaded a big third-party library but that only a small portion of it actually ran.

You can also use DevTools throttling to mimic real-world conditions by turning on network and CPU throttling when measuring third-party impact. On a fast local machine with no throttling, a third-party script might seem to load innocuously in 100 ms. But on a typical midtier smartphone on slow 4G, that could be 500 ms or more plus slower execution. By testing under “Regular 4G” and a fourfold CPU slowdown (which approximates a low-end device), you’ll see what users really experience. Many third-party scripts (especially heavy analytics and visual widgets) have a disproportionate impact under constrained conditions, so it’s crucial to measure with throttling for an accurate picture.

The Performance panel recording can also reveal long tasks caused by third-party script execution. DevTools even supports a long task API in pages themselves: you can use PerformanceObserver to log long tasks in real time, which can include identifying the culprit frame or script. Some advanced RUM setups use this to track third-party slowdowns on real user devices.

## WebPageTest (WPT)

WPT is a powerful ally in understanding third-party impact. It can produce a handy waterfall chart showing the percentage of requests and bytes coming from each domain on your page. Figure 8-2 shows an example of such a chart. If you see that, say, 50% of the bytes are from *.doubleclick.net or *.facebook.com, you know that third-

Another valuable WPT feature is request blocking, which allows you to simulate the page load with certain domains blocked. A recommended workflow is this:

1. Run a test normally (no blocking) to get baseline load times.
2. Go into Advanced settings. In the “Block” field, list the hostnames of known third-party domains (like ad networks) to block.
3. Run a second test blocking those domains.
4. Compare the two results using WPT’s filmstrip and comparison features.

By doing this A/B test, you can quantify the impact of those third parties. For example, you might find that the page visually completes two seconds faster without the ad scripts. That delta is essentially the “performance tax” of those scripts. Andy Davies, a web performance expert, has demonstrated using WPT comparisons to measure third-party costs and found it very insightful for prioritizing what to optimize or remove.

Chrome DevTools lets you perform similar experiments: in the Network panel, you can right-click a request and choose “Block request domain.” This will prevent any request to that domain from loading, and you can then reload the page to see the effect. While this is a manual test (and won’t give you a filmstrip like WPT does), it’s a quick way to check during development: “What if I remove this tag? Does my load time improve dramatically?” Remember to disable caching in DevTools when doing this to simulate first-load conditions.

WPT also has a special SPOF simulation. Instead of outright blocking a domain, SPOF mode simulates what happens if that domain is slow or times out. This is useful because third-party problems often manifest as slow servers rather than complete failures. Using the SPOF tool, you can see, for instance, how your page behaves if the analytics server takes five seconds to respond. Does it delay the whole page? Does the page eventually load without it? This helps you assess risk and ensure you have proper timeouts and fallbacks.

By thoroughly auditing with the tools and methods covered in this section, you will gain a deep understanding of your site’s third-party landscape. Keep logs of these findings: they’ll inform the strategies you choose and help you track improvements as you begin optimizing.

## Principles of Third-Party Optimization

Before I dive into any specific techniques, it’s useful to establish some guiding principles and a mindset for optimizing third-party scripts. Unlike your own code, you can’t edit an analytics script to make it execute faster—it’s someone else’s code. So the kind of optimization you’ll do here revolves around how and when you load those scripts and how you manage their impact on the page.

When you’re dealing with a third-party script that hurts performance, you have two options, broadly speaking: you can get rid of it (by removing it or replacing it with a more efficient alternative), or you can optimize its loading and execution to minimize its interference with your page’s critical path. This includes deferring it, loading it asynchronously, conditionally loading it, and otherwise managing its execution timing and environment. A holistic strategy may involve a bit of both: maybe removing a few tags that aren’t needed and heavily optimizing the rest.

## Remove, Reduce, Replace

Now that you have a list of third-party inclusions and their costs, start with a critical look at each one. What does it do? Is it worth it? Is it truly needed? The most effective optimization is elimination. There’s no script that loads faster than the script you don’t load at all. So part of measuring impact is arming yourself with data to make the case: “This chat widget adds 300 KB and 0.5 s load, but only one in a thousand users clicks it. Perhaps we should load it differently or remove it.”

It’s easy for sites to accumulate third-party scripts over time (from various teams and experiments) and never remove them. Periodic audits often find
