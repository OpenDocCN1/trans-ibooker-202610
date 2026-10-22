```
title TEXT,
link TEXT
);""

⑤
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

⑥
c.execute("SELECT link FROM NewsFeeds WHERE link=?", [link])
link_exists = c.fetchone()
if link_exists:
    # Record already extists in the database
    pass
else:
    newdata = (title, link)
    c.execute("INSERT INTO NewsFeeds (title, link) VALUES (?,?)\",\
    newdata)
⑦
print("Added to DB: " + title)

conn.commit()
conn.close()
```

Here’s a quick review of the new code we added to the script.

+   ① Import the sqlite Python library.

+   ② Check to see if the feeddata.db database file already exists. If this is the first time we run the script, we’ll need to create it.

+   ③ Connect to the feeddata.db database file. Python will automatically create an empty feeddata.db file for us if one doesn’t already exist.