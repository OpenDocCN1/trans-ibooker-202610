# Next Steps

Whew! That was a long project, but it was worth it. You now have a fully functional, audio-generating RSS reading application that can be tailored to whatever well-formed RSS feeds you want to parse. And because you’re storing past feed results in a database, only new articles since the last time you ran the pispeak.py script will be compiled into a new MP3 file for listening at home, work, or on the go.

Improvements are guaranteed with any software endeavor, and this project is no different.

The TTS technology called upon by the gtts library is far better than the tinny robotic voices used by open source TTS engines like Festival,[61] but they still sound unnatural. Advanced machine learning is being used by major tech companies like Google, Microsoft, and Mozilla to generate high-quality TTS translations that sound much more like a speaking person. Using the sophisticated open source Mozilla TTS project[62] can make a huge difference in natural sounding text translation quality. Unfortunately, while installing and configuring Mozilla’s TTS engine on a Pi 4 is possible, it is by no means as simple as running a pip3 install command. Read the Wiki on the Mozilla TTS project page for more details.

Another necessary improvement would be to wrap each phase of the workflow in try/except/finally blocks to handle exceptions. If an exception occurs, attempt to handle it programmatically or, at the very least, send an email indicating what kind of failure occurred.

For those who prefer a more stimulating challenge, construct a custom podcast client script that not only monitors what was downloaded and processed but what was also played back via the Pi’s HDMI or analog headphone jack output. Mark what was listened to and either archive or delete the MP3 files that are no longer needed.