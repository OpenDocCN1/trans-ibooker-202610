though: they don’t mess with the DOM much beyond adding their own elements, which frameworks will ignore if outside their root element (the DOM element where the framework is mounted, like a <div id="root">) or if using nonclashing IDs.

SPAs—which load once and update content dynamically via JavaScript—and MPAs—which load new pages from the server on each navigation—have slightly different challenges. In an SPA, when navigating between “pages” (views) without a full page reload, you might need to manage adding and removing third-party scripts for certain views. For instance, a marketing embed might only be needed on the home page. In an MPA, you’d include the embed’s script tag only on the home page. In an SPA, if the user goes to the home page, you might need to load that script dynamically and perhaps unload it when leaving. Unloading JavaScript is tricky, but you’d basically remove event listeners or elements, if possible.

Fortunately, there are some framework-based solutions. In an SPA, Next.js still loads each page’s scripts according to the strategy, even on client transitions. But after a client-side transition, the new page’s beforeInteractive scripts can’t run retroactively before hydration, which happened when the app launched. Next.js solves that by loading critical things up front or limiting such usage to the initial load by design.

If you’re using React Router or similar, you might use an effect in page components. For example:

```
useEffect(() => {
  if (!window.thirdPartyLoaded) { /* load it */ }
  return () => { maybe cleanup }
}, [])
```

Some frameworks, like Qwik, aim to pause and resume logic and handle third-party differently. Qwik actually integrates Partytown for third-party scripts by default. In a vanilla SPA, however, you have to be careful not to double-load scripts. If a script is needed globally, consider loading it once at app start (with async) so it’s available.