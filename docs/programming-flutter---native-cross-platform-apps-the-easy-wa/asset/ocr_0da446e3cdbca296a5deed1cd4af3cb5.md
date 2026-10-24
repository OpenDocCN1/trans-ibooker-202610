Now we need to be able to reach that page from the home page. We’ll do this by using AppBar actions, which are widgets we can show on the right side of the app bar. In this case, we’re going to use an IconButton:

```
appBar: AppBar(
  title: Text(title),
  actions: <Widget>[
    IconButton(
      icon: Icon(Icons.looks_one),
      tooltip: "Select Comics by Number",
      onPressed: () =>
        Navigator.push(
          context,
          MaterialPageRoute(
            builder: (BuildContext context) => SelectionPage()
          ),
        ),
      ),
    ],
  ),
```

+   ① This is where we are going to add a List of widgets to show on the AppBar. In this case it’s going to be just one IconButton.

+   ② The IconButton also needs a tooltip to help the user understand what the button does. Further on in the book there is a screenshot of what it looks like.

+   ③ This is just like the ComicTile’s onpressed.

The updated home page, showing the overall new look of the app, looks like:

![](bbox=[0.15, 0.66, 0.29, 0.86])

and when the user keeps the button pressed the tooltip will look like: