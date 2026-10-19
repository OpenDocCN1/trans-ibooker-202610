
# The Telegraph Media Group

I’ve referenced the *Telegraph*’s journey throughout this book. To summarize, the *Telegraph*, a major British news publisher, undertook a project to improve its web performance, focusing heavily on third-party scripts used for advertising and analytics.

## Problem

The *Telegraph* discovered that its own first-party code accounted for only about 5% of the requests made during page load. The overwhelming majority came from third-party scripts used for advertising, analytics, social media integrations, and other services. These third parties were outside the team’s direct control but were having a significant negative impact on the site’s loading performance. The challenge was compounded by the fact that different teams across the organization owned relationships with different third-party vendors, making coordination difficult.

## Optimizations

*Telegraph* engineer Gareth Clubb emphasized that initially, the team’s own code optimizations only affected about “5% of the requests.” The rest were third parties outside their direct control. This realization shifted the *Telegraph* engineering team’s focus to broader organizational involvement. The team created a performance task force and managed to defer almost all third-party scripts.

## Results

Not only did site speed improve, but the *Telegraph*’s business metrics did not suffer. In fact, its case study observed that “We can say with confidence though that deferring our JavaScript hasn’t skewed any existing analytics and it certainly hasn’t delayed any advertising. By using custom performance marks in the advertising code, deferring JavaScript and reducing bundle sizes, the First Ad Loaded metric improved by an average of four seconds.” This is a powerful validation that speeding up the site did not harm ad revenue; it likely improved user engagement so much that everything still tracked fine.

The *Telegraph*’s approach combined technical deferral with cross-team collaboration to review tags regularly. It’s a prime example of culture and technique working hand in hand to achieve success.

# The Economic Times: Passing CWV at Scale

The *Economic Times* (ET) is one of India’s largest news websites.

## Problem

In 2021, many of the ET’s pages failed Google’s CWV criteria, which could potentially hurt its search rankings. ET’s pages are content-heavy and include third-party elements like ads and social embeds. Initially, only around 62% of page views were in the “Good” CLS range; about 26% were “Poor” (CLS over 0.25). LCP was around 4.5 s at the 75th percentile, also poor. The team identified slow server response, unoptimized fonts, and late-loading ads as key issues.

## Optimizations

Over nine months, ET’s development team overhauled the site’s performance, including:

### Optimizing server response (TTFB)

ET switched to a faster DNS provider and added a caching layer (using Redis) for frontend templates, so repeat visits or heavy traffic surges could be served from cache instead of hitting the origin server each time.

### Putting “critical requests first”

ET prioritized critical above-the-fold content in the loading sequence. This involved deferring scripts that weren’t needed for initial render, like analytics and below-the-fold widgets, as well as using `<link rel="preload">` for key resources like large images and critical CSS. ET also reduced the number of concurrent requests by bundling where possible, since the browser can only handle so many at once.

### Switching the font-loading strategy

Text rendering was being delayed by custom web fonts. By changing from default `font-display:auto` to `font-display:swap`, the team allowed the text to show in a system font immediately then swap to the custom font when ready. This eliminated blank text flashes and made content readable faster, improving both LCP and CLS (since late font swaps can shift layout).

### Using Brotli compression

On the server, the ET team switched from GZip to Brotli compression for text assets. Brotli made JavaScript, HTML, CSS, and font files 15% to 18% smaller, reducing download times.

### Preconnecting strategically

The team audited third-party domains and added `<link rel="dns-prefetch">` or `preconnect` for those that are always needed. This saved precious time in establishing network connections. However, the team was cautious to use preconnect only for domains that would certainly be used to avoid wasting resources.

### Code splitting and deferring scripts

The ET team broke its JavaScript bundles into smaller chunks, loading only what was needed for initial content and deferring the rest to later. ET also deferred any unused scripts; for example, heavy analytics or slideshow scripts were loaded after the main content.

### Reserving space for ads and embeds

A major source of ET’s CLS problem was advertisements loading in and pushing content, so the team created styled placeholders with dimensions for ad slots and other embed containers to prevent layout jump.

### Adding explicit image dimensions

Similar to ads, ET added explicit width and height attributes or CSS for all images and video containers. This was crucial to eliminate cumulative shifts on article pages with many inline images that loaded progressively.

### Optimizing long tasks (FID/INP)

While the primary focus was on LCP and CLS, the team also broke up long JavaScript tasks to improve interactivity (FID). The team used techniques like requestIdleCallback and splitting heavy loops into smaller chunks. The team deferred loading third-party scripts until after user interaction if possible, such as lazy loading social share buttons. The team also lazy loaded below-the-fold ads so that the main thread wasn’t tied up loading ads while the user was trying to interact. Finally, the team trimmed polyfills for old browsers to reduce JavaScript payloads.

## Results

By July 2021, ET had achieved substantial performance improvements:

- User engagement metrics improved dramatically: ET reported an overall 43% reduction in bounce rate after the changes. In other words, far fewer users landed on a page and left immediately—likely because the pages now loaded quickly and without jarring shifts, encouraging users to stay and read.
- LCP “Poor” occurrences fell by 33%, and average LCP improved about 80%, from 4.5 s to 2.5 s at p75.
- CLS “Poor” occurrences dropped by 65% (from 25.9% of sessions to just 9%), and average CLS went from 0.25 to 0.09, a 90% improvement that places this metric well within the “good” zone.
- Internally, the team also noted better pages per session and time on site. The smoother experience, especially on mobile (where networks are slower), kept readers on the site longer.
- Consequently, the site as a whole passed all CWV thresholds, which was a big deal for ET’s SEO and for Google’s page experience update.

The *Economic Times* case shows that by attacking performance holistically (network, server, and frontend), even a large news site can achieve fast, app-like performance and reap the benefits in user retention.

# Yahoo! Japan News: Correlating CLS Fixes to Engagement

Yahoo! Japan News is one of Japan’s largest news platforms, with around 22 billion page views a month. By fixing a CLS issue on article pages, the company saw a clear improvement: page views per session went up ~15%, session duration went up 13%, and bounce rate decreased.

## Problem

Through continuous monitoring, the team noticed that pages with poor CLS were hurting user engagement. Using Google Search Console’s CWV report, Yahoo’s engineers pinpointed that the article detail page template was frequently in the “poor” bucket for CLS (Figure 12-1). Yahoo then used Lighthouse’s CLS audit (“Avoid large layout shifts”) to find which DOM elements were moving around. This revealed that certain components—specifically the article header area and ad slots within articles—were causing significant shifts.

Yahoo also instrumented the site with the web-vitals JS library to measure CLS in the field and see exactly when in the page lifecycle shifts occurred (including user scroll-triggered shifts). This combination of tools gave a full picture: the primary offenders were likely images or ads loading without reserved space.
