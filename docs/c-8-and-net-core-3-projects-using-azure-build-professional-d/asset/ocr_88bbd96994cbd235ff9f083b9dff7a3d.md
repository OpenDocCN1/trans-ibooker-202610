As you can see, we’re passing a method to the command as an argument. Before we look at the method itself, we need to go on a bit of a detour; otherwise, the method won’t make much sense.

# # DirectLineWrapper

Ultimately, the purpose of this entire application is to send a message to our bot and print the reply on the screen. The next thing that we’re going to introduce is a wrapper class to do just that. We’ll fill in the actual class code later in the chapter, but here, we’re just going to use the class as though it does everything we need. Let’s start by declaring it as a class-level variable:

```
BotClientSdk.DirectLineWrapper _wrapper = null;
```

Now, we’ll instantiate this in the constructor, which should now look like this:

```
public MainViewModel()
{
    SendMessageCommand = new RelayCommandAsync<string>(SendMessage);
    _wrapper = new BotClientSdk.DirectLineWrapper(PopulateHistory);
}
```

You’ll notice that we’re passing a variable into the DirectLineWrapper class; we’ll come back to this method shortly.

Broadly speaking, instantiating a dependency inside a constructor is bad practice. It makes testing the class very difficult; should you wish to extend this project, moving this into an injected dependency would be a good start.

The SendMessage method body should be changed as follows:

```
private async Task SendMessage(string message)
{
    await _wrapper.SendMessage(message);
    MessageText = string.Empty;
}
```

The _wrapper has a method called SendMessage. This calls a method inside a class that we’ll create shortly. Finally, we clear the text box so that the user can enter another message.