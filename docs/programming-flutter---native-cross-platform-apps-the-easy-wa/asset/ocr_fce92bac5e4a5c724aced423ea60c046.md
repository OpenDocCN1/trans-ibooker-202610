```
"or isn't available"
)
],
}
);
}
}
```

# Make Your Own Changes

We have already seen quite a lot of Dart code, but have you been writing your own as well? If you haven't, here is another chance (after the one in Wrapping Up the Calculation) for you to make a very simple and slight improvement to our XKCD app.

The ErrorPage we just wrote generically informs the user that we couldn't get that comic. This can also be used any time we can't fetch a comic (for example, if there is no Internet available and the cache was wiped).

You can do this yourself very easily by adding a snapshot.hasError condition to each FutureBuilder. You could go one step further and compare the comic number given to the SelectionPage to the latest comic number and show a different error if the user accidentally gave a comic number that is bigger than the latest comic number.