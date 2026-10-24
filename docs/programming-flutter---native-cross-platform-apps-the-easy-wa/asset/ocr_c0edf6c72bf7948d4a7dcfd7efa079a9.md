```
decoration: InputDecoration(
  labelText: "Insert comic #",
),
keyboardType: TextInputType.number,
autofocus: true,
onSubmitted: (String a) => Navigator.push(
  context,
  MaterialPageRoute(
    builder: (context) => FutureBuilder(
      future: _fetchComic(a),
      builder: (context, snapshot) {
        if(snapshot.hasData) return ComicPage(snapshot.data);
        return CircularProgressIndicator();
      }
    ),
  ),
),
```

① This is a TextField, which is a widget that allows the user to input text.

② keyboardType is used to set the type of information the user is supposed to input (the framework will optimize the display and/or the type of keyboad shown to the user to suit this setting). It is of type TextInputType, which can be one of the following:

+   - TextInputType.text (default) for regular single-line text.
+   - TextInputType.multiline for multiple lines of text.
+   - TextInputType.number, which is what we're going to use, for numbers.
+   - TextInputType.datetime for date and time.
+   - TextInputType.emailAddress for email addresses.
+   - TextInputType.phone for telephone numbers.

③ This argument makes the view automatically focus on the TextField. This means the keypad will automatically open and any numbers typed on it will be sent to the TextField, without requiring the user to tap on it first.

④ We haven't yet fetched the comic, which means we need to call the FutureBuilder to fetch the comic and use the comic as an argument for the ComicPage.