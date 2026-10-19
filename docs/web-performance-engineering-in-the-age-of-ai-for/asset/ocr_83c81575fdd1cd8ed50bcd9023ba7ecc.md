# Summarizing Sequencing Best Practices

In summary, best practices for sequencing third-party scripts include:

*Prioritize first-party content*

Always load your site’s core content and critical resources before noncritical third-party scripts. Structure the HTML accordingly.

*Place scripts late*

Put third-party script tags at the bottom of the body or load them dynamically after content, unless it’s absolutely necessary to do otherwise.

*Use async and defer*

Mark third-party scripts as `async` or `defer` so they don’t block the parser. Use `defer` for most cases, especially if there are multiple scripts or if a script must run after the DOM is ready. If order matters, rely on `defer` (or manage load via callbacks). Avoid relying on chance with `async` ordering.

*Load critical third-party scripts early*

For the rare third-party script that’s critical, still load it asynchronously, but do so as early as needed (deferred in the head). Ask if it truly *must* be head-loaded.

*Preconnect if possible*

Use `<link rel="preconnect">` for key third-party domains to reduce connection-setup time.

*One at a time (maybe)*

Don’t flood the page with a dozen third-party loads all at once, especially early on. Even async scripts can contend for bandwidth, if many start downloading simultaneously. Stagger them if possible by using priorities or initiating some later.