Let’s quickly review the code used in this script before we attempt to run it.

+   ① Import the BeautifulSoup and feedparser Python libraries.
+   ② Store the three RSS feed URLs that will be parsed in a static URLS array.
+   ③ Assign a single url from each URL stored in the URLS array, and process that RSS feed url by having feedparser parse the feed elements that can be later queried upon.
+   ④ For each entry in the feed being parsed, extract the title, link and description elements.
+   ⑤ Rather than just store the raw HTML description, remove the embedded HTML tags so they’re not rendered when passing this string to the text-to-speech parser later in this project. Doing so will also make the output more human-readable as well.
+   ⑥ Combine the results of the title, link, and HTML-cleaned description into a string with appropriate line breaks, and print() out the results.

Save and run the feedtest.py file and execute it with the Python interpreter:

```
$ python3 feedtest.py
```

Assuming your code syntax is correct and your Pi is connected to the Internet, you should see a list of article titles and associated web links.

You now have a script that will quickly print out the latest newsfeed articles that have been posted to websites selected for this demonstration. That’s pretty nifty. You also completed the project’s first objective.

Next, you need to identify which of those listed articles were added since the last time the feedresults.py script was executed. Otherwise, you would have to listen to news items you previously heard sprinkled with any additions made