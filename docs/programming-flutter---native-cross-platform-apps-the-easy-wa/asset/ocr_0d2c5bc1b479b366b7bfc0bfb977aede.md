# The List of Messages

In the MessagesList widget, we’ll take the list of messages and create a ListView using the ListView.builder constructor, passing the data needed to show the message (sender, text, and timestamp) to a Message widget we’ll build later.

The ListView’s reverse argument makes the list *start from the bottom*, meaning the first element will be shown at the bottom, which will be the scrolling view’s default position, which will be held when new messages are added. This is the typical behavior of chat apps and we want to replicate it, so we’ll set that to `true`:

```
```dart
class MessagesList extends StatelessWidget {
  MessagesList(this.data);

  final QuerySnapshot data;

  bool areSameDay(Timestamp a, Timestamp b) {
    var date1 = a.toDate().toLocal();
    var date2 = b.toDate().toLocal();
    return
      (date1.year == date2.year)
      &&
      (date1.month == date2.month)
      &&
      (date1.day == date2.day);
  }

  @override
  Widget build(BuildContext context) =>
      ListView.builder(
        reverse: true,
        itemCount: data.documents.length,
        itemBuilder: (context, i) {
          var months = [
            "January",
            "February",
            "March",
            "April",
            "May",
            "June",
            "July",
            "August",
            "September",
            "October",
            "November",
            "December"
          ];
          DateTime when = data
              .documents[i]
              .data["when"]
              .toDate()
              .toLocal();
          var widgetsToShow = <Widget>[
```
```