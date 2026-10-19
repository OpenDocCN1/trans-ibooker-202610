not harm ad revenue; it likely improved user engagement so much that everything still tracked fine.

The *Telegraph*’s approach combined technical deferral with cross-team collaboration to review tags regularly. It’s a prime example of culture and technique working hand in hand to achieve success.

# The Economic Times: Passing CWV at Scale

The *Economic Times* (ET) is one of India’s largest news websites.

# Problem

In 2021, many of the ET’s pages failed Google’s CWV criteria, which could potentially hurt its search rankings. ET’s pages are content-heavy and include third-party elements like ads and social embeds. Initially, only around 62% of page views were in the “Good” CLS range; about 26% were “Poor” (CLS over 0.25). LCP was around 4.5 s at the 75th percentile, also poor. The team identified slow server response, unoptimized fonts, and late-loading ads as key issues.

# Optimizations

Over nine months, ET’s development team overhauled the site’s performance, including:

*Optimizing server response (TTFB)*

ET switched to a faster DNS provider and added a caching layer (using Redis) for frontend templates, so repeat visits or heavy traffic surges could be served from cache instead of hitting the origin server each time.

*Putting “critical requests first”*

ET prioritized critical above-the-fold content in the loading sequence. This involved deferring scripts that weren’t needed for initial render, like analytics and below-the-fold widgets, as well as using `<link rel="preload">` for key resources like large images and critical