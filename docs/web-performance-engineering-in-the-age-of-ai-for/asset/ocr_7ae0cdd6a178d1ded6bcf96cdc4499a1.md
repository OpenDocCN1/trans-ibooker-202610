CSS. ET also reduced the number of concurrent requests by bundling where possible, since the browser can only handle so many at once.

# Switching the font-loading strategy

Text rendering was being delayed by custom web fonts. By changing from default `font-display:auto` to `font-display:swap`, the team allowed the text to show in a system font immediately then swap to the custom font when ready. This eliminated blank text flashes and made content readable faster, improving both LCP and CLS (since late font swaps can shift layout).

# Using Brotli compression

On the server, the ET team switched from GZip to Brotli compression for text assets. Brotli made JavaScript, HTML, CSS, and font files 15% to 18% smaller, reducing download times.

# Preconnecting strategically

The team audited third-party domains and added `<link rel="dns-prefetch">` or `preconnect` for those that are always needed. This saved precious time in establishing network connections. However, the team was cautious to use preconnect only for domains that would certainly be used to avoid wasting resources.

# Code splitting and deferring scripts

The ET team broke its JavaScript bundles into smaller chunks, loading only what was needed for initial content and deferring the rest to later. ET also deferred any unused scripts; for example, heavy analytics or slideshow scripts were loaded after the main content.

# Reserving space for ads and embeds

A major source of ET’s CLS problem was advertisements loading in and pushing content, so the team created styled placeholders with