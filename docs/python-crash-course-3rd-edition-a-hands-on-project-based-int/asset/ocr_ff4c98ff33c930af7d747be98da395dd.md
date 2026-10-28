```
'sarah': 'c',
'edward': 'rust',
'phil': 'python',
}
```

As you can see, we’ve broken a larger dictionary into several lines. Each key is the name of a person who responded to the poll, and each value is their language choice. When you know you’ll need more than one line to define a dictionary, press ENTER after the opening brace. Then indent the next line one level (four spaces) and write the first key-value pair, followed by a comma. From this point forward when you press ENTER, your text editor should automatically indent all subsequent key-value pairs to match the first key-value pair.

Once you’ve finished defining the dictionary, add a closing brace on a new line after the last key-value pair, and indent it one level so it aligns with the keys in the dictionary. It’s good practice to include a comma after the last key-value pair as well, so you’re ready to add a new key-value pair on the next line.

NOTE
*Most editors have some functionality that helps you format extended lists and dictionaries in a similar manner to this example. Other acceptable ways to format long dictionaries are available as well, so you may see slightly different formatting in your editor, or in other sources.*

To use this dictionary, given the name of a person who took the poll, you can easily look up their favorite language:

*favorite_languages.py*