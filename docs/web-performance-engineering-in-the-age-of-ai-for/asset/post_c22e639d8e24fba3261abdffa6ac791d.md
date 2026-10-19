
and metrics you can use to measure that impact, then finish the chapter by introducing a set of principles to guide you as you decide how best to optimize their presence.

## Third-Party Code Is Everywhere

Nearly every modern website uses at least one third-party script in some form—over 94%, according to the HTTP Archive Web Almanac. In fact, the median website on desktop pulls from 23 third-party domains, and on mobile the median is 21. On highly popular sites, the count is even higher: according to the same study, the top 1,000 sites use 40 to 47 third-party integrations on average. And a study in the Web Almanac notes that about 33% of the total scripts loaded on an average site are third-party scripts. They’re ubiquitous—not edge cases but a core part of the web ecosystem. Table 8-1 shows some of the most common use cases for third-party scripts.

Table 8-1. Third-party scripts fulfill many purposes across different types of sites

| Type of third-party content | Example | Script function/purpose |
|---|---|---|
| Analytics and tracking | Google Analytics, Mixpanel, Facebook Pixel | Log user behavior and traffic sources; measure conversions and remarketing |
| Advertising networks | Google AdSense, programmatic ad networks | Bidding, ad rendering, tracking impressions |
| Media embeds | YouTube, Vimeo, Instagram, SoundCloud | Play back and display video and audio content |
| Social sharing widgets | Facebook “Like” button | Social media interaction |
| Chat and support widgets | Intercom, Zendesk | Render the chat widget; handle messaging |
| Tag managers | Google Tag Manager | Manage analytics, A/B testing, and advertising tags centrally |
| A/B testing and personalization | Optimizely or Google Optimize | Swap out or modify page elements for different test variants; personalize content |
| Utility libraries | date-fns, Moment.js | A date-picker library, a video player library from a CDN |
| Ecommerce and SaaS integrations | Shopify, Discus | Reviews, recommendation widgets, analytics, external comment systems |

Across web apps, content management system (CMS) sites (like WordPress), and ecommerce platforms (like Shopify), the patterns are similar: third-party scripts offer plug-and-play capabilities for common needs. A Shopify merchant might add an app for an upsell pop-up (which injects a script), while a WordPress blogger might paste an embed code from SoundCloud to share audio. Modern single-page applications (SPAs) also use third-party SDKs. For instance, a React app might dynamically load the Google Maps API for a map component or use Stripe’s JS library for payments. In short, third-party scripts are everywhere, in every category of site.

## Why Third-Party Scripts Affect Performance

Unfortunately, the convenience of third-party code comes with significant performance impacts. Introducing scripts that the browser must fetch from various origins and execute extends the critical path of page loading beyond your own code. Key reasons third-party scripts tend to slow down web pages include:

*Extra network requests*

Each third-party resource often requires additional HTTP requests to different servers. This means extra DNS lookups, TLS handshakes, and

data transfers. Every new domain (analytics, ads, etc.) adds latency. As the web.dev team puts it, “the more requests a site has to make, the longer it can take to load.” A page might load its own code quickly from its server but then stall while waiting on multiple calls to ad networks, analytics endpoints, and content delivery networks (CDNs) for third-party content.

# Large script payloads

It’s not uncommon for a single embed to pull in hundreds of kilobytes or even a couple of megabytes of JavaScript: a typical social-media feed embed or video player can easily include as much as 500 KB of script. This can dominate the total bytes of the page. Users on slower connections or with limited data plans could pay a high cost to download code that isn’t even part of your site’s core functionality.

# Main-thread execution cost

Parsing and executing JavaScript on the browser’s main thread can delay interactivity. As you learned in Chapter 7, when the browser encounters a script, by default it must pause parsing the HTML to execute the script’s code. Third-party scripts often run sizable code blocks on load, which can block your own page from rendering. Even if loaded asynchronously, a large script can still hog the CPU once it runs. These “boot-up” times add to the time to interactive (TTI) and can cause jank. Analysis from web.dev shows that third-party scripts frequently block the main thread, delaying user interactions. If the script does heavy work (like analytics processing or building a widget UI), it may introduce noticeable lag or even frame drops.

# Contention for resources

Third-party content competes with first-party content for bandwidth and CPU. If an advertisement script starts making lots of network calls or doing layout, it’s competing with your own images and scripts for the main thread (and other resources). Without careful sequencing, third-party resources can consume network resources that could otherwise be

fetching important images or API data for your main app scripts or CSS, potentially delaying initial render or painting.

# Render-blocking

Some third-party embeds, especially older ones, use outdated techniques that outright block the page from rendering. Certain ad or tracking scripts still use the notorious document.write(), which can halt parsing and insert content synchronously, leading to performance hazards. Even when using modern async techniques, third-party iframes can delay the window.onload event if their servers respond slowly. In other words, an external script might be loading “asynchronously” but still end up gating the load event or other page milestones due to how browsers treat slow iframe loads.

When you include several third-party widgets, there’s also a cumulative impact. Each widget might bring along its own miniframework or library. For instance, you might include a date-picker library in your own code, and then an A/B testing script separately injects its own copy of a similar library—doubling the bytes and work. It’s possible to end up loading jQuery multiple times or loading two incompatible versions of a framework because two embeds each fetch their own copy. This duplication wastes resources and exacerbates performance issues.

Third-party code can also be unpredictable. Since it’s fetched from the third party’s server each time, it can change without you knowing or load additional scripts of its own (so-called fourth-party scripts). Andrew Welch notes that often, when you include a “tag” (really a snippet of third-party JavaScript), it can essentially do whatever it wants inside the user’s browser, including loading further scripts. For example, a tag manager might load five tags, each of which then calls out to other services, ballooning the number of requests. This cascade effect can degrade performance in ways that are hard to anticipate or monitor.

Relying on external servers introduces the risk that if one of them is slow or down, your page might hang or break. This is known as the single point of

*failure* (SPOF) problem. If a third-party server doesn’t respond or times out, it could block parts of your site from loading. In the worst case, a crucial third-party script (like a CDN-hosted library) failing to load could even halt your site’s functionality.

Third-party embeds often inject content (like an iframe of unknown size) into your page asynchronously. A classic example is an ad that loads in after the article text, moving the text. Such shifts hurt the user experience. If you don’t account for them by reserving space for them in advance, this can push your content around, causing layout shifts; in fact, many third-party iframes are significant contributors to poor CLS (covered in Chapter 2).

While not a performance issue per se, it’s worth noting that third-party scripts can introduce security vulnerabilities by opening the site to potential cross-site scripting (XSS) attacks, as well as privacy issues by tracking users or accessing sensitive data. Third-party tags should be reviewed not just for speed but also for compliance with privacy regulations and security.

In summary, third-party scripts make the web more dynamic, interactive, and interconnected—but using them is risky. They often execute outside the site owner’s control and can significantly impact page load speed and stability. With so much code coming from elsewhere, performance optimization can’t ignore this portion of the stack.

I recall a WebPageTest (WPT) trace of a news site that showed that the tracking and marketing scripts took longer to load than the page’s own content. The network waterfall revealed that third-party requests dominated the timeline. Figure 8-1 shows an example of a domain breakdown chart where a huge percentage of the bytes and requests were from third-party domains. This kind of data is a wake-up call: without intervention, third-party scripts can steal the spotlight from your first-party content, leaving users waiting.

Given these stakes, web performance experts treat third-party script optimization as a critical task. In the following chapters, I’ll explore how to analyze the impact of third-party scripts and optimize their loading, execution, and overall footprint.
