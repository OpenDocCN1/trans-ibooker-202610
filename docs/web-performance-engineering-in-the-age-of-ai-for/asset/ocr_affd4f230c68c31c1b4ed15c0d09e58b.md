In this snippet, the loadThirdPartyScripts() function might dynamically load ads or large analytics, but could be skipped for very slow connections or data-saver users.

Even without the Network Information API, you can approximate some information, though this indirect method isn’t ideal. If the initial page load takes a very long time (you might measure this by TTFB), that might hint at a slow network. Alternatively, you could provide users a manual toggle like “Basic mode,” but that’s more UI complexity.

# Trade-Offs

Like all of the techniques discussed here, conditional loading has its trade-offs. Here are its advantages:

+   - The experience is better for users on slower networks or less powerful devices, as they get a lighter, faster page.
+   - Data savings are available for users with limited bandwidth or pay-per-use plans.
+   - Core Web Vitals metrics are improved across diverse user segments.
+   - You can still provide alternative contact methods or simpler versions of features, ensuring no user is completely left out.

Downsides include:

+   - It’s hard to perfectly detect capability. Some midrange phones might still struggle; some fast connections have high latency; but the network might be congested even though the effectiveType says '4g'. So your rules won’t be perfect. Aim to cover the clear extremes (very slow, very low-end) and be moderate with others.
+   - Testing can be complex, since you essentially create multiple possible code paths for different users. It’s important to test