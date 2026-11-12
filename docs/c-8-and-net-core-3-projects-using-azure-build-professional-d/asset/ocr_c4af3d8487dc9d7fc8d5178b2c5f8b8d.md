The second piece of data is held in a special collection, known as an
ObservableCollection.

# ObservableCollection

An ObservableCollection notifies the UI when its content changes. That is, every time
you add or remove an item, the UI will automatically be updated. An
ObservableCollection in every other respect behaves like any other collection (say a
List).

> It's worth bearing in mind that, while an ObservableCollection will
> update the UI when you add or remove an item, it will not update the UI
> if you change the collection itself. For example, look at the following:

```
MyCollection = new ObservableCollection<MyObject>()
```

This would not update the UI. Furthermore, changing the contents
of MyObject (in this case) would not update the UI.

For now, we won't worry about ChatHistory, although by now you've probably worked
out what it will look like. Next, we'll have a look at the command.

# Command binding

In the UI, we had a button that had its Command property set. In this section, we won't have
a look at what the Command actually looks like. In this code, we'll be using a wrapper class
called RelayCommand. We'll come back to RelayCommand shortly, but a command is
simply any class that implements the ICommand interface. The helper class that we're
using here just means that we can declare the command functionality inside the
ViewModel.

Initially, we just declare the RelayCommand:

```
public RelayCommandAsync<string> SendMessageCommand { get; set; }
```

Then, we instantiate this inside the constructor:

```
public MainViewModel()
{
    SendMessageCommand = new RelayCommandAsync<string>(SendMessage);
}
```