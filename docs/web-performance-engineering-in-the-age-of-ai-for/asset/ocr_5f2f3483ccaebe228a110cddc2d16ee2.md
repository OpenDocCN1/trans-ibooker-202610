the page is being unloaded, such as when the user navigates away, closes the tab, or switches to another application) to capture final metric values before the page session ends. It applies any necessary polyfills or fixes, like for INP support in browsers that might not have it natively yet, and it provides a simple callback with the metric value and some metadata.

To use web-vitals, run the following script:

```
<script src="https://unpkg.com/web-vitals@3/dist/web-vitals.iife.js"></script>

<script>
  webVitals.getCLS(console.log);
  webVitals.getLCP(console.log);
  webVitals.getINP(console.log);
</script>
```

This logs the metrics to console. To send them to a server, run the following:

```
function sendToAnalytics(metric) {
  const body = JSON.stringify(metric);
  // Use navigator.sendBeacon for efficient async send that doesn't block unload
  navigator.sendBeacon('/analytics', body);
}

webVitals.getCLS(sendToAnalytics);
webVitals.getLCP(sendToAnalytics);
webVitals.getINP(sendToAnalytics);
```