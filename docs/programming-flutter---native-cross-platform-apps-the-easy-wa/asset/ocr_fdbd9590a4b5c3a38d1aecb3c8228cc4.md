```
_battSub.cancel();
}
```

You can find out more about Streams in Streams and StreamSubscriptions.

# DEFINING THE APP’S UI

The build method will be the following:

packages/battery_connectivity/lib/main.dart

```
@override
Widget build(BuildContext context) {
  return Scaffold(
    backgroundColor: Theme.of(context).backgroundColor,
    appBar: AppBar(
      title: Text(widget.title),
    ),
    body: Column(
      mainAxisAlignment: MainAxisAlignment.spaceAround,
      children: <Widget>[
        Row(
          children: <Widget>[
            Icon(_connectionIcon, color: _connectionColor, size: 50.0),
            Text(
              "Connection:\n$_connectionStatus",
              style: TextStyle(
                color: _connectionColor,
                fontSize: 40.0,
              ),
            ),
          ],
        ),
        Row(
          children: <Widget>[
            Icon(_batteryIcon, color: _batteryColor, size: 50.0),
            Text(
              "Battery:\n$_batteryStatus",
              style: TextStyle(
                color: _batteryColor,
                fontSize: 40.0,
              ),
            ),
          ],
        ),
      ],
    );
  }
}
```

The app’s body is a Column, with the mainAxisAlignment set to MainAxisAlignment.spaceAround so the widgets occcupy the entire body, instead of being stacked closely on top:

packages/battery_connectivity/lib/main.dart

```
body: Column(
  mainAxisAlignment: MainAxisAlignment.spaceAround,
```