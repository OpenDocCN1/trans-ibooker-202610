```
children: <Widget> [
  CircleAvatar(
    backgroundImage: NetworkImage(
      "https://pragprog.com/favicon.png"),
    ),

  Padding(
    padding: EdgeInsets.all(20),
    child: Text("Short poster"),
  ),
],
),
Align(
  alignment: FractionalOffset.centerLeft,
  child: Padding(
    padding: EdgeInsets.all(5),
    child: Text("This is a short post, but that doesn’t really matter"),
  ),
),
Divider(),
Row(
  children: <Widget> [
    IconButton(
      icon: Icon(Icons.thumb_up, color: Colors.redAccent),
      onPressed: () {},
    ),
    IconButton(
      icon: Icon(Icons.comment, color: Colors.greenAccent),
      onPressed: () {},
    ),
    IconButton(
      icon: Icon(Icons.share, color: Colors.blueAccent),
      onPressed: () {},
    ),
  ],
),
],
),
```

To create a complete view with a ListView of posts we should also store the post data separately in a List, since in a real app you’re never going to hard-code such data.

To do that, we’ll start by creating a class to contain the post data:

```
class Post {

  Post({this.profileName, this.profilePicture, this.postText});

  ImageProvider profilePicture;
  String profileName;
  String postText;
}
```

and a List of that class with some data:

```
List<Post> posts = [
  Post(
```