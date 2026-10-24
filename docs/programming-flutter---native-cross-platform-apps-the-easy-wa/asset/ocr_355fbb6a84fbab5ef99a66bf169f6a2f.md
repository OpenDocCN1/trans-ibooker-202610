```
postText: "This is a short post, but that doesn’t really matter",
profileName: "Short poster",
profilePicture: NetworkImage("https://pragprog.com/favicon.png"),
),
Post(
postText: "This is a longer post, which really shows how the" +
"widgets will naturally expand when there is more content to display.\n" +
"At this point, this line of text is just here because we need to" +
"make this post so much bigger than the other one.",
profileName: "Long poster",
profilePicture: NetworkImage("https://pragprog.com/favicon.png"),
),
];
```

At this point, the resulting ListView is quite straightforward:

```
ListView.builder(
  itemCount: 2,
  itemBuilder: (context, i) => Card(
    child: Column(
      children: <Widget> [
        Row(
          children: <Widget> [
            CircleAvatar(
              backgroundImage: posts[i].profilePicture,
            ),
            Padding(
              padding: EdgeInsets.all(20),
              child: Text(posts[i].profileName,
                style: Theme.of(context).textTheme.subhead),
            ),
          ],
        ),
        Align(
          alignment: FractionalOffset.centerLeft,
          child: Padding(
            padding: EdgeInsets.all(5),
            child: Text(posts[i].postText,
              style: Theme.of(context).textTheme.body1),
          )
        ),
        Divider(),
        Row(
          children: <Widget> [
            IconButton(
              icon: Icon(Icons.thumb_up, color: Colors.redAccent),
              onPressed: () {},
            ),
            IconButton(
              icon: Icon(Icons.comment, color: Colors.greenAccent),
              onPressed: () {},
            ),
            IconButton(
              icon: Icon(Icons.share, color: Colors.blueAccent),
              onPressed: () {},
            ),
          ],
        ),
      ],
    ),
  ),
```