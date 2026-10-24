After logging in, the user has to enter a display name, so we need a screen called ConfigPage specifically meant to achieve that, since it’s a step that requires authentication to have happened and is a prerequisite to using the chat page:

```
firebase/chatonfire/lib/main.dart
```

```
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(title: Text("Configure you account's basic information")),
    body: ListView(
      children: <Widget>[
        TextField(decoration: InputDecoration(
          labelText: "Display Name"
        ),
        onSubmitted: (displayName) =>
          setNameAndGoToChatPage(displayName, context),
        controller: _controller,
        ),
        FlatButton(
          child: Text("Submit"),
          onPressed: () =>
            setNameAndGoToChatPage(_controller.text, context),
        )
      ],
    );
  }
}
```

Once again, we’re going to implement setNameAndGoToChatPage later. All we need to know about it is that it’s certainly going to push a new screen, loading a widget called ChatPage, which we’ll implement now.

# Chat Page

Here’s what we’re going to build in this section:

![](bbox=[0.148, 0.676, 0.302, 0.89])