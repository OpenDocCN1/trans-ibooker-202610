# OTHER ITERABLE DATA STRUCTURES: MAPS, SETS, AND MORE

The Map is a key/value collection, similar to JSON objects and often used as the way to handle JSON objects in Dart as a simpler alternative to defining a class for each kind of object the app works with. A Map literal is enclosed in curly braces and looks something like this:

```
var user = {
  "userId": 1,
  "firstName": "Carmine",
  "lastName": "Zaccagnino"
};
```

You can access and add elements to a Map by using the same [] and [] you’d use to access and modify elements of a List:

```
var name = "${user['firstName']} ${user['lastName']}";
user["password"] = "my!!Very!!own!!Very!!safe]passWordstr";
```

A Set a linear collection of different values:

```
var G8 = {
  "United States",
  "Japan",
  "Germany",
  "United Kingdom",
  "France",
  "Italy",
  "Canada",
  "Russia"
};
```

You can remove an item from a Set by value using remove and add one using add:

```
var G7 = G8
  ..remove("Russia")
  ..add("European Union");
```

Set Literals