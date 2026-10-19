data transfers. Every new domain (analytics, ads, etc.) adds latency. As the web.dev team puts it, “the more requests a site has to make, the longer it can take to load.” A page might load its own code quickly from its server but then stall while waiting on multiple calls to ad networks, analytics endpoints, and content delivery networks (CDNs) for third-party content.

# Large script payloads

It’s not uncommon for a single embed to pull in hundreds of kilobytes or even a couple of megabytes of JavaScript: a typical social-media feed embed or video player can easily include as much as 500 KB of script. This can dominate the total bytes of the page. Users on slower connections or with limited data plans could pay a high cost to download code that isn’t even part of your site’s core functionality.

# Main-thread execution cost

Parsing and executing JavaScript on the browser’s main thread can delay interactivity. As you learned in Chapter 7, when the browser encounters a script, by default it must pause parsing the HTML to execute the script’s code. Third-party scripts often run sizable code blocks on load, which can block your own page from rendering. Even if loaded asynchronously, a large script can still hog the CPU once it runs. These “boot-up” times add to the time to interactive (TTI) and can cause jank. Analysis from web.dev shows that third-party scripts frequently block the main thread, delaying user interactions. If the script does heavy work (like analytics processing or building a widget UI), it may introduce noticeable lag or even frame drops.

# Contention for resources

Third-party content competes with first-party content for bandwidth and CPU. If an advertisement script starts making lots of network calls or doing layout, it’s competing with your own images and scripts for the main thread (and other resources). Without careful sequencing, third-party resources can consume network resources that could otherwise be