```
}

class HomePage extends StatelessWidget {

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Home Page")),
      body: Center(
        child: Container(
          height: 100,
          child: Card(
            child: Column(
              children: <Widget>[
                Padding(
                  padding: EdgeInsets.all(10.0),
                  child: Text(
                    "Click the button to go to a new page",
                    style: Theme.of(context).textTheme.title,
                  ),
                ),
                ButtonTheme.bar(
                  child: FlatButton(
                    child: Text("Go to new page"),
                    onPressed: () {}
                  ),
                ),
              ],
            ),
          ),
        ),
      );
    }
  }
}
```

which generates the following app in its initial state as shown in the screenshot.

![](bbox=[0.15, 0.6, 0.33, 0.85])

I made the home page a bit more complex than it needed to be to differentiate the two pages visually.