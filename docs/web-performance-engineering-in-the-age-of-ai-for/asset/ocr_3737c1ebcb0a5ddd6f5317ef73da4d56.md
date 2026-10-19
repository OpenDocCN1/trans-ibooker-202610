textual content. Use these techniques only for nonprimary content so that critical content is still indexed.

# Analytics impact

If you use scrolling-based lazy loading, your analytics may still need to account for when a lazy-loaded component actually appears or is viewed, if that matters. Some analytics tools don’t automatically know about content loaded later.

# Browser differences

As noted, different effective connection types might cause different behavior. Chrome’s native lazy loading might load a bit earlier on a fast connection than it would on slow 3G. If consistency is key, use IntersectionObserver (with your own logic) or `lazysizes` to make behavior uniform across conditions.

# Example: Lazy Loading an Analytics Script

Thus far, I’ve spoken of lazy loading visible widgets. But you can lazy load even nonvisual scripts like analytics in some cases. If your site’s primary content is very performance-sensitive, you might choose to lazy load Google Analytics—meaning you don’t load it until after the main content is shown or even until the user interacts (since Google Analytics can technically miss initial page view timing if loaded late, one must weigh trade-offs).

A pattern some use is to load analytics with `defer` (so it’s after parse), or even wait for the `load` event (using `window.onload = function() { loadAnalytics() }`). This is a form of lazy loading by event. It ensures that the analytics script doesn’t interfere with page load, though it might miss capturing some early data, like the page’s own load timing. Many modern analytics actually can be loaded deferred without issue, but it’s something to consider.