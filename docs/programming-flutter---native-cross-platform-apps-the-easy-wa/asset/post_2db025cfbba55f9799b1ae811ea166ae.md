
"or isn't available"
)
],
}
);
}
}

## Make Your Own Changes

We have already seen quite a lot of Dart code, but have you been writing your own as well? If you haven't, here is another chance (after the one in Wrapping Up the Calculation) for you to make a very simple and slight improvement to our XKCD app.

The ErrorPage we just wrote generically informs the user that we couldn't get that comic. This can also be used any time we can't fetch a comic (for example, if there is no Internet available and the cache was wiped).

You can do this yourself very easily by adding a snapshot.hasError condition to each FutureBuilder. You could go one step further and compare the comic number given to the SelectionPage to the latest comic number and show a different error if the user accidentally gave a comic number that is bigger than the latest comic number.

## Permanent Data I/O in Flutter:
Adding “Starred” Comics

Up until now we have only used the temporary cache directory, but if we want to add a feature that allows the user to save some comics to local storage permanently as a way to keep their favorite comics always available to them. To do that, we need to have access to a permanent local storage facility.

That is available to us in the form of application documents directory, which is where an app is supposed to save data that should be kept until the user consciously deletes it.

We can get the path of the application documents directory using getApplicationDocumentsDirectory, and we can use it in the same way we use the cache directory we got through getTemporaryDirectory: we use getApplicationDocumentsDirectory().path as part of the path we give to the File’s constructor.

Let’s get to implementing this feature, starting with adding a star-shaped button in the app bar in the home page to access previously saved comics and one in the comic page to save comics.

But even before we do any of that, we need to turn the ComicPage into a StatefulWidget if we want the view to reflect the change in the comic’s state triggered by the user’s action. This means we need to do two things: change the definition of the ComicPage to be a subclass of StatefulWidget and turn what was up until now the ComicPage into a State<ComicPage> definition, inside which the same build method will reside:

```
networking/xkcd_app/lib/main.dart

class ComicPage extends StatefulWidget {
  ComicPage(this.comic);

  final Map<String, dynamic> comic;

  @override
```

```
_ComicPageState createState() => _ComicPageState();
}

class _ComicPageState extends State<ComicPage> {
```

```
@Override
Widget build(BuildContext context) {
  // return Scaffold(...)
}
}
```

The next step is to decide what kind of file we should use for storing the starred comics. We have used two different kind of files until now:

- A JSON-encoded text file to store the comic data and a simple text file to store the latest comic number.
- A binary file to save the image data.

The choices have been dictated by the nature of the data we needed to save: images are binary data, whereas we get the comics in a textual JSON format, meaning they can easily be saved in that format to local storage. A single number can be stored as binary data or text-based data without too many differences.

The starred comics will be a list of numbers, and there are advantages and disadvantages to each approach:

- Saving in a binary format (using `writeAsBytes`) is simpler to implement and requires less computing overhead, but will generate files that are not readable by the user, but more efficient on storage.
- Saving it in a JSON list makes it possible for the user to read and edit the file directly, but results in a less efficient use of computing power and storage space.

We are going to use binary format in this case since that seems to be the optimal solution for this problem. We’ll start by implementing an `_addToStarred` in the `ComicPageState` to add a comic to the starred comics list:

```
networking/xkcd_app/lib/main.dart

void _addToStarred(int num) {
  var file = File("$docsDir/starred");
  List<int> savedComics = json.decode(
    file.readAsStringSync()
  ).cast<int>();
  if(isStarred) {
```

```
savedComics.remove(num);
}
else {
  savedComics.add(num);
}
file.writeAsStringSync(json.encode(savedComics));
}
```

which depends on the existence of isStarred as a ComicPage member variable:

networking/xkcd_app/lib/main.dart

```
bool isStarred;
```

We need to determine whether or not the comic is starred when we load the ComicPage for the first time, since we will be changing that variable’s value whenever the user taps on the star button. Since we can’t have an async initState, we’ll use .then:

networking/xkcd_app/lib/main.dart

```
void initState() {
  super.initState();
  getApplicationDocumentsDirectory().then(
    (dir) {
      docsDir = dir.path;
      var file = File("$docsDir/starred");
      if(!file.existsSync()) {
        file.createSync();
        file.writeAsStringSync("[]");
        isStarred = false;
      }
      else {
        setState((){isStarred = _isStarred(widget.comic["num"]);});
      }
    }
  );
}
```

This depends on an onStarred method that finds out if the comic have been starred by opening the file containing the starred comic numbers, casting it from a List of dynamic (which is a data type that can contain anything) to a List of integers and checking whether the comic number is contained in the list:

networking/xkcd_app/lib/main.dart

```
bool _isStarred(int num) {
  var file = File("$docsDir/starred");
  List<int> savedComics = json.decode(
```

```
file.readAsStringSync()
).cast<int>();
if(savedComics.indexOf(num) != -1)
  return true;
else
  return false;
}
```

## ADDING THE APPBAR ACTION TO THE COMIC PATH

Now we need to implement a way to add each comic to the starred. The way we’ll do it in this case is by showing a star-shaped icon in the app bar that, when tapped, calls the _addToStarred method on the current comic, adding it to the starred comics list. To do that, we’ll add yet another AppBar action to our app in the ComicPage’s build method:

networking/xkcd_app/lib/main.dart

```
actions: <Widget>[
  IconButton(
    icon: isStarred == true ?
      Icon(Icons.star) :
      Icon(Icons.star_border),
    tooltip: "Star Comic",
    onPressed: () {
      _addToStarred(widget.comic["num"]);
      setState() {
        isStarred = !isStarred;
      };
    }
  ),
],
```

This is what our new ComicPage looks like:

![](img/eb93f09b5f47b8f6c9bead1b9047c506_253_0.png)

and what its app bar will look like if the current comic has been starred:

![](img/eb93f09b5f47b8f6c9bead1b9047c506_253_1.png)

```
file.writeAsStringSync("[]");
} else {
  json.decode(file.readAsStringSync()).forEach(
    (n) async =>
      comics.add(await _fetchComic(n.toString()))
  );
}
return comics;
}
```

- ① Here we define the needed variables and fetch the application documents directory and use it to define the file which we will be operating on.
- ② We need to check whether the starred comics file exists; if it doesn’t, we just create the file and end up returning an empty list (we’ll work with that later when we build the UI for the starred comics page).
- ③ If the file exists, we fetch all of the comics listed in the file and return them.

Now we’ll build the UI for the *StarredPage*:

```
networking/xkcd_app/lib/starredPageUI.dart
```

```
@override
Widget build(BuildContext context) {
  ① var comics = _retrieveSavedComics();

  return Scaffold(
    appBar: AppBar(
      title: Text("Browse your Favorite Comics")
    ),
    body: FutureBuilder(
      future: comics,
      builder: (context, snapshot) =>
        ② snapshot.hasData && snapshot.data.isNotEmpty ?
          ListView.builder(
            itemCount: snapshot.data.length,
            itemBuilder: (context, i) =>
              ComicTile(comic: snapshot.data[i]),
          )
        ③ :
```

```
Column(
  children: [
    Icon(Icons.not_interested),
    Text("""
      You haven't starred any comics yet.
      Check back after you have found something worthy of being here.
    """),
  ]
)
```
