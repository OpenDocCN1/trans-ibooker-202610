# Audio and Video Generation or Text to Speech (TTS)

AI-generated media like TTS audio or video synthesis can stream progressively, much like adaptive streaming for traditional video content. Key metrics are time to first playable frame or audio chunk (how quickly does playback start), stall frequency (does playback pause while waiting for more data), and buffer underrun count (how often does the client run out of data).

Techniques to try include choosing a segment size that matches the network’s variability. Too large, and the user waits a long time before anything plays; too small, and you waste overhead on frequent network round trips. Keep a small client-side buffer (a second or two of media) to smooth over network hiccups, but prefer low-latency codecs to minimize the time between generation and playback. For truly real-time applications (like voice assistants), prioritize low latency over high quality. Users prefer an immediate, slightly lower-quality response over a high-quality one that takes noticeably longer.

# Images That Arrive Progressively

AI image generation services often produce images over several seconds. You can stream these progressively (like progressive JPEGs) or in tiles. Reserve space for these up front. Use width and height attributes or CSS to reserve the image’s space before it arrives, preventing layout shift. Use loading="lazy" and fetchpriority="high" on hero images to prioritize what matters.

Progressive or tiled delivery is also an option. If your image generator supports it, stream a low-resolution preview first, then progressively refine it. Or deliver the image in tiles (like map tiles) if it’s very large. This gives users something to look at immediately, even if the final high-res version takes time. Use opacity or filter transitions to fade in refined images smoothly, avoiding jarring snaps from blurry to sharp.