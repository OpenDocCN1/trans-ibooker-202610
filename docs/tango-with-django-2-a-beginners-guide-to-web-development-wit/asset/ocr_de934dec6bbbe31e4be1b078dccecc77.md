to a variable definition like the example below. Make sure you put this underneath the definition of `BASE_DIR`!

# Delete those Lines!

If you included the three `print()` statements above to see what’s going on, make sure you remove them once you understand. Don’t just leave them lying there. They will clutter your settings module, and clutter the output of the Django development server!

```
TEMPLATE_DIR = os.path.join(BASE_DIR, 'templates')
```

Here, we make use of the `os.path.join()` function to join together (or *concatenate*) the value of the `BASE_DIR` variable, and the string `'templates'`. Upon completion, this concatenation yields `<workspace>/tango_with_django_project/templates/`. From here, we can then use our new `TEMPLATE_DIR` variable to replace the hard-coded path we defined earlier in `TEMPLATES`. Update the DIRS key/value pairing to look like the following code snippet.

```
'DIRS': [TEMPLATE_DIR, ]
```

# Why `TEMPLATE_DIR`?

You’ve created a new variable called `TEMPLATE_DIR` at the top of your `settings.py` file because it’s easier to access should you ever need to change it. For more complex Django projects, the DIRS list allow you to specify more than one template directory to draw templates from. For this book however, one location is sufficient to get everything working.

# Concatenating Paths

**When concatenating system paths together, always use `os.path.join()`.** Using this built-in function ensures that the correct path separators are used. On a UNIX operating system (or derivative of), forward slashes (`/`) would be used to separate directories, whereas a Windows operating system would use backward slashes (`\`). If you manually append slashes to paths, you may end up with path errors when attempting to run your code on a different operating system, thus reducing your project’s portability.