# Nonblocking Scripts: async and defer

Using the `async` or `defer` attributes on script tags are the primary ways to load JavaScript without blocking the HTML parser. Let’s clarify these attributes:

# defer

With `defer`, the script is fetched in parallel while the document is parsing, but its execution is deferred until after the HTML parsing is complete. Deferred scripts also preserve execution order relative to each other: if you have two scripts with defer, they will execute in the order they appear in the document (after parsing).

# async

With `async`, the script is fetched in parallel and executed as soon as it’s downloaded and available, without waiting for HTML parsing to finish. Critically, async scripts do *not* guarantee execution order: two async scripts can finish and execute out of order if one downloads faster. When an async script executes, it can still pause the parser, but at least downloading it doesn’t block the parser initially. In modern browsers, async works a bit differently for module scripts, which will defer execution to maintain order among dependencies. For classic scripts, though, think of async as “download in the background, execute ASAP.”

For third-party scripts, I generally recommend using `defer` by default for noncritical scripts (any script that is not needed before the DOM is ready). This ensures it won’t interfere with HTML parsing and will run after the document is done, which is usually what you want for things like analytics or ads. In contrast, `async` is useful for scripts that you want to load in parallel and run as soon as possible but that aren’t dependent on other scripts. For example, if you have an early analytics snippet that you want to start recording data immediately, you could use `async` so that it fires