```
print('Hiding ' + submission.title)
await submission.hide()
```

Once again, we’re limiting the number of new submissions being iterated upon to 20. If the submission being queried is visible (in other words, it’s hidden value is False), then inform the user by printing to the Terminal that it is being hidden and then wait for that submission to actually be set to hidden.

Save and run the script in the usual way:

```
$ python3 reddhide.py
```

As each new visible submission is processed, it’ll be displayed accordingly and then set to hidden. Refresh the new tab on the user account’s Reddit home page and you should see those new submissions processed by the reddhide.py script are no longer showing up on that web page. If you run the reddhide.py script again, only those new submissions posted since the last time you ran the script will be displayed. They’ll also be marked as hidden.

Now that we know how to programmatically list and hide new submissions in subscribed subreddits, we can transplant this code into Discord bot code that can transform this bland Terminal output experience into a fun, interactive one.