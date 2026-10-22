
the apt command from the Terminal.

```
$ sudo apt install sqlitebrowser
```

I’ll be predominantly using this application to display screenshots of this project’s queries, due to its ease of use and clean tabular presentation of data.

Lastly, if you’re using Microsoft Visual Code as your editor of choice, you can install a popular SQLite extension called, well, SQLite.[60] While not as full-featured as the DB Browser app, it’s functional enough for viewing query results and performing basic SQLite database operations.

With your choice of SQLite viewer installed, we can proceed with adding database creation and storage capability to the code we previously wrote. Create a new Python script file called feeddbtest.py, copy and paste the feedtest.py code, and then add the extra SQLite code to it:

```
pispeak/feeddbtest.py

from bs4 import BeautifulSoup
import feedparser
import os
import sqlite3

URLS = ['https://forum.devtalk.com/latest.rss',
    'https://feeds.npr.org/1001/rss.xml',
    'https://projects-raspberry.com/news-updates/raspberry-pi-news/feed/',
]

if os.path.exists('feeddata.db'):
    read_articles = True
else:
    read_articles = False
    print("feeddata.db database doesn't exist, creating new database file.")
    conn = sqlite3.connect('feeddata.db')
    c = conn.cursor()

    c.execute("""CREATE TABLE NewsFeeds (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
```

```
title TEXT,
link TEXT
);""")

conn.commit()
conn.close()

conn = sqlite3.connect('feeddata.db')
c = conn.cursor()

for url in URLS:
    feed = feedparser.parse(url)

    for entry in feed["entries"]:
        title = entry.get("title")
        link = entry.get("link")
        description = BeautifulSoup(entry.get("description"), "lxml").text

c.execute("SELECT link FROM NewsFeeds WHERE link=?", [link])
link_exists = c.fetchone()
if link_exists:
    # Record already exists in the database
    pass
else:
    newdata = (title, link)
    c.execute("INSERT INTO NewsFeeds (title, link) VALUES (?,?)", newdata)

print("Added to DB: " + title)

conn.commit()
conn.close()
```

Here’s a quick review of the new code we added to the script.

- ① Import the sqlite Python library.
- ② Check to see if the feeddata.db database file already exists. If this is the first time we run the script, we’ll need to create it.
- ③ Connect to the feeddata.db database file. Python will automatically create an empty feeddata.db file for us if one doesn’t already exist.

④ Create a new table called NewsFeeds in our feeddata.db database with fields for id, title, and link.

⑤ commit the database table changes to the feeddata.db database file and close the connection to make those changes permanent.

⑥ Execute a SELECT statement to see if we already stored the record in the database before. If we did, there’s no reason to store it again.

⑦ If this is in fact a new record that we haven’t seen before, we’ll add it to the database using the INSERT statement. Then we commit any changes made and close the connection to the feeddata.db database.

Save the feeddbtest.py file and run it:

```
$ python3 feeddbtest.py
```

Since this is the first time you’ve run the script, it will create a new feeddata.db database file and then populate the NewsFeeds table with the most recent title and link entities retrieved from the RSS feeds. If you try running the script again after the script’s initial successful run, the script will see that the feeddata.db file already exists, so it won’t re-create or overwrite the NewsFeeds table and the data stored within it. And unless there was a newsfeed that updated its content in between your script executions, there will likely be no new data added to the database either.

To see what data was stored in the NewsFeeds table, launch the DB Browser for SQLite app (located in the Programming submenu on the Raspberry Pi OS Desktop), then navigate to and open the feeddata.db database file. Select the Browse Data tab and scroll through the data you stored there.

Nice job! You’ve just completed the most challenging portion of the project. Now that you know what new articles are available, you just need to convert them to MP3 audio format, then store that compiled audio file into the

previously designated Rclone’d folder for easy cloud-based file retrieval.

## Converting Audio

Several approaches are available to convert text to speech, but the easiest I’ve come across is an incredibly useful and powerful Python-based library called gtts. You can install it using the pip3 command:

```
$ sudo pip3 install gtts
```

Once installed, you can call its command-line interpreter front end in the Terminal, like any other program.

For example, if you wanted to generate a sample TTS file from this gtts-cli utility, you can do so from the Terminal window:

```
$ gtts-cli "This is a quick test." -o output.mp3
```

Locate the output.mp3 file in the same directory you ran the command, and using the File Manager desktop app, double-click the file to load and play it in the VLC media player.

Now let’s do the same thing programmatically by calling upon the gtts library from a Python script. Using your preferred text editor, create a new file called ttstest.py and enter the following code:

pispeak/ttstest.py

```
① from gtts import gTTS

test_text = 'This is a test of TTS conversion for the PiSpeak project!'

② output = gTTS(text=test_text, lang='en', tld='com', slow=False)

③ output.save("output.mp3")
```

Here’s a brief explanation of what this script does.

- ① Import the gTTS library used to perform the TTS conversion.
- ② Pass the test_text text string along with language and playback speed parameters into the gTTS object, perform the conversion, and save the results to the output object.
- ③ Save the audio content stored in the output object to the output.mp3 audio file.

Save and execute the ttstest.py script. Using the File Browser, open the output.mp3 file located in the same directory from where you ran the script and listen to the playback.

One annoying limitation with the current gtts implementation is its inability to selectively alter the TTS playback speed. I find even after setting gtts’s slow parameter equal to False that the playback result is still too slow for my fast-paced listening preference. Fortunately there’s an easy fix to this problem.

## Processing the Audio

ffmpeg is one of the most important and popular open source utilities for rendering and manipulating audiovisual files. It is what powers the VLC player and numerous other open source and commercial media applications. In addition to being the Swiss Army knife of the media file world, it’s also a tremendously useful tool for converting media files from one format to another. It can also be used to process additional effects on media data such as fades, transitions, and playback speed.

I’m always busy learning new things, with time being a key parameter in life that I can’t change. But I can change how I use that time, as well as how I can use utilities like ffmpeg to change time for the content I consume. If you’ve ever used a modern podcast application, you know that most have the ability to speed up playback without affecting the pitch. In other words, you can listen to the same content in less time without the presenters sounding like chipmunks.

While the audio playback apps I listed in this project’s Software section possess the built-in ability to speed up playback, it does take additional computational overhead on the smartphone to do so on the fly. If you plan on setting the playback speed on the device consistently at the same altered rate, then you’re downloading twice as much data as you need to render the file. To reduce this overhead and take advantage of the available processing time we have on an idle Pi, we can have ffmpeg perform this file processing ahead of time. We’ll do this for the MP3 file we previously generated.

ffmpeg is already installed in the Raspberry Pi OS for Desktop, but if you’re using a different version of the OS, you can install ffmpeg via the apt install command:

```
$ sudo apt install ffmpeg
```
