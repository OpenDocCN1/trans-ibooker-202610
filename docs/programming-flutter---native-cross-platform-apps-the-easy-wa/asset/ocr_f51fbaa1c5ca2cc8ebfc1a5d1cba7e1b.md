```
networking_starting/xkcd_app/lib/main.dart
```

```
Future<Map<String, dynamic>> _fetchComic(int n) async =>
  json.decode(
    await http.read(
      "https://xkcd.com/${latestComic-n}/info.0.json"
    )
  );
```

Now all that’s left to do is the fun part: defining the UI we want for our app in the build method. We’ll start by defining the Scaffold and AppBar, as we always do:

```
networking_starting/xkcd_app/lib/main.dart
```

```
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: Text(title),
    ),
```

```
  );
}
```

The Scaffold’s body will be, as we said, a ListView.builder-built ListView, and we need to give it two arguments:

+   - An itemCount, which is the maximum number of items that will be built; in this case, it will be the latestComic number, since that’s also the number of comics that have ever been published.
+   - An itemBuilder, which is a function called with the BuildContext and an int that starts from 0 at the top (we’ll call it i).

In this case the itemBuilder is going to be, as we said before, a FutureBuilder.

The FutureBuilder, in turn, requires two arguments: a future, which is the Future that will retrieve the data needed for each tile, and a builder function, which builds the tile taking the BuildContext and the result of the Future.

The future will be a call to fetchComic(i) where i is the aforementioned progressive integer variable and the itemBuilder will return a ComicTile that takes the comic data as an argument. We’ll worry about implementing the ComicTile itself later.