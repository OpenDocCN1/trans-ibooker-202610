```
```dart
var file = File('${dir.path}/latestComicNumber.txt');
int n = 1;

try {
  n = json.decode(
    await http.read('https://xkcd.com/info.0.json')
  )["num"];
  file.exists().then(
    (exists) {
      if(!exists) file.createSync();
      file.writeAsString('$n');
    }
  );
}
catch(e) {
  if(file.existsSync() &&
     file.readAsStringSync() != ""
  )
    n = int.parse(file.readAsStringSync());
}
return n;
```
```

The updated code should look as follows:

```
```dart
// testing/xkcd_app/lib/main.dart
Future<int> getLatestComicNumber
({http.Client httpClient, File latestComicNFile}) async
{
  if(httpClient == null) {
    httpClient = http.Client();
  }

  if(latestComicNFile == null) {
    final dir = await getTemporaryDirectory();
    latestComicNFile = File('${dir.path}/latestComicNumber.txt');
  }

  int n = 1;

  try {
    n = json.decode(
      await httpClient.read('https://xkcd.com/info.0.json')
    )["num"];
    latestComicNFile.exists().then(
      (exists) {
        if(!exists) latestComicNFile.createSync();
        latestComicNFile.writeAsString('$n');
      }
    );
  }
  catch(e) {
    if(latestComicNFile.existsSync() &&
       latestComicNFile.readAsStringSync() != ""
    )
      n = int.parse(latestComicNFile.readAsStringSync());
  }
  return n;
}
```
```