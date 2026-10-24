# Building the Comic Page

The ComicPage is going to be the simplest of the widgets we talked about until now: we’re going to give it an AppBar that shows the comic number, and the body is simply going to be a scrollable ListView that shows the comic’s title, image and alt-text:

```
class ComicPage extends StatelessWidget {
  ComicPage(this.comic);

  final Map<String, dynamic> comic;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("#${comic[\"num\"]}")),
      body: ListView(children: <Widget>[
        Center(
          child: Text(
            comic["title"],
            style: Theme.of(context).textTheme.display3,
          ),
        ),
        Image.network(comic["img"]),
        Padding(
          padding: EdgeInsets.all(8.0),
          child: Text(comic["alt"])
        ),
      ]),
    );
  }
}
```

All of that adds up to the following `main.dart`:

```
import 'package:flutter/material.dart';
import 'package:http/http.dart' as http;

import 'dart:async';
import 'dart:convert';

Future<int> getLatestComicNumber() async =>
  json.decode(
    await http.read('https://xkcd.com/info.0.json')
  )
  ["num"];
```