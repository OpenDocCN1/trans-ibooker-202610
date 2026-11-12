```
Message = message;
}
public string Message { get; set; }
public string Sender { get; set; }
}
```

Before we move on, we need to see the RelayCommandAsync class.

# # RelayCommandAsync

The code for RelayCommandAsync is not too onerous. We implement an interface called ICommand, which has just two methods. A bare-bones implementation of ICommand would look like this:

```
public class RelayCommandAsync<T> : ICommand
{
    public bool CanExecute(object parameter)
    {
    }

    public void Execute(object parameter)
    {
    }
}
```

Clearly, this does nothing, so let's fill in those methods. We'll start with Execute; we need to pass an action into the Execute method, and we can do that through the constructor:

```
public class RelayCommandAsync<T> : ICommand
{
    readonly Func<T, Task> _execute = null;

    public RelayCommandAsync(Func<T, Task> execute)
    {
        if (execute == null)
            throw new ArgumentNullException("execute");

        _execute = execute;
    }

    public void Execute(object parameter)
    {
        _execute.Invoke((T)parameter);
    }
}
```