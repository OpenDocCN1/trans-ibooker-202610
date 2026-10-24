

Closely related is the mainAxisSize, which can be either MainAxisSize.max or MainAxisSize.min.

There is also crossAxisAlignment, which sets how the children widgets should be aligned along the cross axis (in the case of a Column it is the horizontal axis) and can be one of the following:

- The default CrossAxisAlignment.center, which aligns the widgets so that their center is aligned.
- CrossAxisAlignment.start and CrossAxisAlignment.end which, respectively, align the widgets to the start or the end of the cross axis.
- CrossAxisAlignment.stretch, which makes the widgets occupy as much space as possible in the cross axis.
- CrossAxisAlignment.baseline, especially useful with Rows, which aligns the children’s baseline.

### What You Use It For

You can use a Column simply, like in the previous chapter, to show two widgets one on top of the other, like this:

```dart
Scaffold(
  appBar: AppBar(...),
  body: Column(
    crossAxisAlignment: CrossAxisAlignment.start,
    mainAxisAlignment: MainAxisAlignment.spaceBetween,
    children: <Widget>[
      Icon(Icons.arrow_upward),
      Text("This will be in the middle"),
      Text("This will be at the bottom")
    ],
  )
)
```

![](img/eb93f09b5f47b8f6c9bead1b9047c506_78_0.png)

but you will probably want to use it more often to group together widgets, perhaps forming a new and more complex widget:

```dart
class TwoTexts extends StatelessWidget {
  TwoTexts({this.firstText, this.secondText});

  String firstText, secondText;

  @override
  Widget build(BuildContext context) {
    return Column(
      children: <Widget>[
        Text(firstText),
        Text(secondText)
      ],
    );
  }
}
```

This will be especially useful inside ListViews, which we’ll see in action now.

## THE LISTVIEW

The ListView is used to create a scrollable list of widgets.

It is used when there is a need to create long lists of similar (or the same) items, and it can dynamically load other elements when they are many (or unlimited).

The list can be expressed in multiple ways, as we will now see.

### How You Use It

There are multiple constructors that you can use to create a ListView.

The default constructor works just like the Column’s constructor: you just need to provide a List of Widgets as the children:

```dart
ListView(
  children: <Widget>[
    Text("1"),
    Text("2"),
    Text("3"),
    ...
  ],
)
```

This example shows the advantage of using another constructor, the ListView.builder constructor, which is used the following way:

```dart
ListView.builder(
  itemCount: 499,
  itemBuilder: (BuildContext context, int i) =>
    Text("${i+1}"),
)
```

This does the same and creates a list of 499 Text widgets with the numbers from 1 to 500.

But this constructor can also be useful when each element has something different—for example, if each Text needs a different, unrelated string.

After defining something like:

```dart
const strings = [
  "The First Widget",
  "The Second Widget",
  "The Third Widget",
  "The Last Widget"
]
```

or generating a List using what is described in Generating Lists.

You can create a ListView like this:

```dart
ListView.builder(
  itemCount: strings.length,
  itemBuilder: (BuildContext context, int i) =>
    Text(strings[i]),
)
```

There is also ListView.separated, which allows you to add a separator widget, in this case yet another Text for simplicity:

```dart
ListView.separated(
  itemCount: 500,
  itemBuilder: (context, i) =>
    Text("$i", style: Theme.of(context).textTheme.display1),
  separatorBuilder: (context, i) =>
    i%3 == 0 ? Divider() : Padding(padding: EdgeInsets.all(0.5),),
)
```

This adds a divider widget[21] once every three numbers, starting from the space between the first and second widget, like in the following screenshot:

![](img/eb93f09b5f47b8f6c9bead1b9047c506_80_0.png)

This is especially useful when you want to display lists of items separated into categories.

Other uses for ListView in general are explained in the Row’s usage section in Where You’ll Use the Row Together with Other Widgets.

shrinkWrap, when set to false (default), will make the list take up as much space as is available on the main axis. If, instead, you set it to true the grid’s size will adapt to the size that is needed to display its children.

### Customizing the Divider

The Divider can be customized to fit your app’s overall look, feel and needs:

- You can set the color option to another color.
- You can set its height option by providing a double number to make the space bigger or smaller.
- You can set its indent option by providing a double number to make the line start at a different position along the horizontal axis.

### Horizontal Lists

You can use scrollDirection to make a horizontally scrolling list, which is very useful if you want to have a horizontal list inside a vertical list. This can be seen in several apps and you’ll see exactly why it is useful.

In the following example, I’ll explain how to show many products, grouped in categories, using a combination of ListView.builder to display the categories and horizontal ListView.builder to show the products. We’ll use example data, defined in the following way:

```dart
const categories = [
  "Automotive",
  "Books",
  "Electronics",
  "Food"
];

const products = [
  [
    "Car", "Tyre", "Fuel", "Oil"
  ],
  [
    "Programming Book", "Novel", "Politics Book", "Business Book"
  ],
  [
    "Desktop Computer", "Laptop", "Computer keyboard"
  ],
  [
    "Pasta", "Pizza", "Bread", "Cheese", "Ham", "Sausage", "Beef"
  ]
];
```

and then render the product list using something like:

```dart
return Scaffold(
  backgroundColor: Colors.greenAccent,
  appBar: AppBar(title: Text("List inside List example")),
  body: ListView.builder(
    itemCount: categories.length,
    itemBuilder: (context, i) =>
      Column(
        children: <Widget>[
          Text(categories[i]),
          Container(
            height: 150.0,
            child: ListView.builder(
              padding: EdgeInsets.all(10.0),
              scrollDirection: Axis.horizontal,
              itemCount: products[i].length,
              itemBuilder: (context, j) =>
                Padding(
                  padding: EdgeInsets.all(20.0),
                  child: Card(
                    child: Padding(
                      padding: EdgeInsets.all(10.0),
                      child: Text(
                        products[i][j],
                        style: Theme.of(context).textTheme.display1,
                      ),
                    ),
                  ),
                ),
            ),
          ),
        ],
      ),
  ),
);
```

which produces the result as seen in the screenshot.

![](img/eb93f09b5f47b8f6c9bead1b9047c506_82_0.png)

The Container is needed to constrain the internal ListView since it is inside another scrollable widget and the app wouldn’t work without it.

More on the Card in The Card.

### The ListTile

The ListTile is the typical widget that is used with ListViews and is very common in chat apps: the typical row made of a (usually round) preview picture, on the left, and on the right side a title and an optional subtitle.

### Implement a ListTile

ListTile is very common and useful and very easy to implement in Flutter:

```dart
ListTile(
  leading: Image(leftImage), // OPTIONAL
  title: Text(title),
  subtitle: Text(subtitle), // OPTIONAL
  onTap: () => openTile(), // OPTIONAL
)
```

leading is the little preview widget shown before the title, and it is usually an image, title is the big text description of the element, the subtitle is smaller and shown below, and onTap is the function that will be fired when the user taps on the tile.

There is also a (less common) trailing argument available to add a widget placed after the title.

For example, the following code, using the list icon as the leading icon:

```dart
ListView.builder(
  itemBuilder: (context, i) =>
    ListTile(
      leading: Icon(Icons.list),
      title: Text("title number ${i+1}"),
      subtitle: Text("subtitle"),
    ),
)
```

will produce:

![](img/eb93f09b5f47b8f6c9bead1b9047c506_84_0.png)

### Make Round Widgets

You previously learned that the leading widget of a ListTile is usually round, but how can you make a round widget?

You build round widgets in two ways: using a CircleAvatar or using a BoxDecoration for a Container.

### Use a Container to Make Widgets of Any Shape and Size

The most versatile and generic way is to contain the widget within a circular shape using a circle-shaped Container.

As an example of the usage of the container, the following code produces a normal square box with an icon and some text:

```dart
Container(
  height: 50.0,
  width: 50.0,
  color: Colors.blueAccent,
  child: Column(
    children: <Widget>[
      Icon(Icons.apps),
      Text(
        "Test",
        style: TextStyle(color: Colors.white),
      ),
    ],
  ),
)
```

just like in the following screenshot:

