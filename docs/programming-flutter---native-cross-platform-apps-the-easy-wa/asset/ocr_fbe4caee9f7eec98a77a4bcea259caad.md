```
),

Center(
  child: Text(
    "Swipe right",
    style: Theme.of(context).textTheme.display1
  ),
),
],
),
)
```

which produces a simple view with an AppBar and, in the middle of the screen, a Text widget.

There is no other on-screen indication that the view has other pages, but swiping left moves the view to the next page and swiping right moves the view to the previous page.

In addition to the children, the PageView has a controller attribute, which can be set to a PageController.

The PageController has a useful option: the initialPage, which is an integer specifying the page (starting from 0) at which the view starts when the app loads.

# TOP NAVIGATION WITH A TABBAR AND TABBARVIEW

Significantly more useful and common is the TabBarView which, paired with a TabBar below the AppBar, provides the same swiping experience of a PageView combined with a nice menu at the top to select which page to switch to and to highlight the page the user is currently at.

The TabBarView and the TabBar are linked by a TabController.

An elegant way to create one is by wrapping the entire view in a DefaultTabController, and setting the entire Scaffold as its child, the number of tabs as its length, and (optionally) an initialIndex (starting from 0, which is the default).

The TabBar can be set as the AppBar's bottom option and it has a tab option where you'll add the Tab widgets, made up of a text and an optional icon, which will be the menu items which will make up the TabBar.