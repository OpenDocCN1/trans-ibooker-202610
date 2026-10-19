lags, a user might tap it twice in frustration, accidentally purchasing two items.

Layout instability can cause errors too: if content shifts suddenly due to slowly loading ads or images, users might click the wrong thing (like a moving link or button), derailing their intended task.

If your app feels slow to respond, users will notice—and many will take action, whether that’s hammering the interface, leaving your site, or mentally checking out. This is why you need to measure and improve performance carefully.

# Evaluating UX Performance: Key Metrics

No single metric tells the whole story of a page load. The user’s experience is a progression of moments, not one instant in time. Historically, developers have looked at metrics like *onload* (the moment when all resources finish loading) or simple stopwatch timings, but these often fail to capture what users actually care about. For example, a page might technically finish loading in 5 s, but if the content is visible and usable in 2 s, the user’s *perceived* performance is much better than the onload time would suggest. Conversely, a page might show something at 2 s but remain unresponsive until 10 s due to heavy JavaScript, yielding a bad experience.

This section provides a quick high-level overview of what performance metrics are and why they’re important. Chapter 2 will dive deeper into the details of this topic, including defining various metrics that are named briefly here.

Developers used to focus primarily on technical measures of performance, but these didn’t always reflect the users’ experience. I approach this by looking at four key moments in a loading experience that correspond to users’ intuitive questions as the page loads:

*Is it happening?*

Did the site respond, or is my connection dead? This is the earliest stage. The user has clicked a link or entered a URL and wants