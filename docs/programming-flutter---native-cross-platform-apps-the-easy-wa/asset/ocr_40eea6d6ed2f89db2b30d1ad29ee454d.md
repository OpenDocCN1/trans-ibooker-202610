This method works just like the older one when no additional named arguments are provided (they would be equal to null, triggering the fallback if clauses), which would happen during normal app use. If they are provided, though, they are used in place of the real file and HTTP client.

Obviously fetchComic also needs to change. Let’s start with the SelectionPage’s, which was the following in the original app:

# networking/xkcd_app/lib/main.dart

```
Future<Map<String, dynamic>> _fetchComic(String n) async {
  final dir = await getTemporaryDirectory();
  var comicFile = File("${dir.path}/$n.json");

  if(
    await comicFile.exists() &&
    comicFile.readAsStringSync() != ""
  )
    return json.decode(comicFile.readAsStringSync());
  else {
    comicFile.createSync();
    final comic = json.decode(
      await http.read('https://xkcd.com/$n/info.0.json')
    );

    File('${dir.path}/$n.png')
      .writeAsBytesSync(await http.readBytes(comic["img"]));
    comic["img"] = '${dir.path}/$n.png';
    comicFile.writeAsString(json.encode(comic));

    return comic;
  }
}
```

and needs to become the following to allow for testing by using the clients and files given as arguments if they are given and reverting back to the default when they aren’t (in normal app usage) just like we did for getLatestComicNumber:

# testing/xkcd_app/lib/main.dart

```
Future<Map<String, dynamic>> fetchComic
(
  String n,
  {
    http.Client httpClient,
    File comicFile,
    File imageFile,
    String imagePath
```