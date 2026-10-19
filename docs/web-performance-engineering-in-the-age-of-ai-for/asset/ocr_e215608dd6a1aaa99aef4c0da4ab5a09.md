# Automatic performance tuning

The system learns optimal settings for each website, automatically adjusting compression levels, HTTP/2 priorities, and early hints based on measured performance improvements.

# Results

Websites leveraging Cloudflare’s AI-powered features reported:

+   - TTFB reductions of 15% to 25% due to improved cache hit rates
+   - More consistent performance during traffic spikes, with 99th percentile load times improving by up to 40%
+   - Reduced origin server load, enabling sites to handle three to five times more traffic without infrastructure upgrades

# Google Chrome: AI-Assisted Speculation Rules

Building on the Speculation Rules API showcased in the Ray-Ban case study, Google has been developing AI-powered prediction systems to automatically determine which pages to prerender.

# Problem

While the Speculation Rules API enables powerful prerendering capabilities, implementing effective speculation logic requires analyzing user behavior patterns, managing memory constraints on devices, and balancing the cost of wrong guesses against the benefit of correct ones. Most development teams lack the analytics infrastructure and expertise to implement optimal speculation strategies.

# Optimizations

Google’s approach uses machine learning to:

+   - Analyze click patterns across millions of websites to identify common navigation behaviors, such as when search result pages