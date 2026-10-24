```
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
  ],
),
```

The Column contains two Rows, one showing the connection state, and one showing the battery state.

# Showing the States

The connection state will be shown using a Row to show both the string and the icon of the color we set when the state changes:

packages/battery_connectivity/lib/main.dart

```
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
```

packages/battery_connectivity/lib/main.dart

```
Row(
  children: <Widget>[
    Icon(_batteryIcon, color: _batteryColor, size: 50.0),
    Text(
      "Battery:\n$_batteryStatus",
      style: TextStyle(

```