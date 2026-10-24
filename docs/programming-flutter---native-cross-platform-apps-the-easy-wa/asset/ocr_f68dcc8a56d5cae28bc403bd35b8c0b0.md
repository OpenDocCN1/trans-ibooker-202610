```
Message(
  from: data.documents[i].data["from"],
  msg: data.documents[i].data["msg"],
  when: when
);
if(i == data.documents.length-1) {
  widgetsToShow.insert(
    0,
    Padding(
      padding: const EdgeInsets.symmetric(vertical: 10.0),
      child: Text(
        "${when.day} ${months[when.month-1]} ${when.year}",
        style: Theme.of(context).textTheme.subhead,
      ),
    );
  } else if(
    !areSameDay(
      data.documents[i+1].data["when"],
      data.documents[i].data["when"]
    )
  ) {
    widgetsToShow.insert(
      0,
      Padding(
        padding: const EdgeInsets.symmetric(vertical: 10.0),
        child: Text(
          "${when.day} ${months[when.month-1]} ${when.year}",
          style: Theme.of(context).textTheme.subhead
        ),
      )
    );
  }
  return Column(
    children: widgetsToShow
  );
}
```

You might have noticed that we are building the ListView using a Column that allows us to show the date on which a message was sent if it is different than the previous message’s date. This is the preferred approach of many chat apps since it allows us to show the date only when it matters to the user.

One disadvantage is that doing it that way is a bit unintuitive and verbose: we need to have an if and an else if necessarily because having both conditions in the first if will cause the app to crash if we try to access the document at index documents.length, but it’s made even worse by the fact that the list is created in two steps, making the contents less clear at first glance.