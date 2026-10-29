+   - Prefer using a “here” document (with leading TABs) to a bunch of echo lines
+   - Use `source` (instead of `.`) to include config files, which should end in `.cfg` (or `.conf` or whatever your standard is)
+   - If at all possible use ISO-8601 ² dates for everything
+   - If at all possible keep lists of things (e.g., ip addresses, hostnames, packages to install, whatever) in alphabetical order; this prevents duplication and makes it easier to add or remove items
+     - This applies to `case` statements, contents of variables or arrays/lists, and wherever it makes sense
+   - If possible, use long arguments to commands for readability, e.g., use `diff --quiet` instead of `diff -q`, though watch out for portability to non GNU/Linux systems
+     - If any options are short or obscure, add comments.
+     - Also strongly consider documenting why you chose or needed the options you chose, and even options you considered but didn’t use for some reason.
+     - Definitely document any options that might seem like a good idea but that actually can cause problems, especially if they are options you commonly use elsewhere.

For variable naming, watch out for generic variable names like:

```
`${GREP} ${pattern} ${file}`
```

That’s very abstracted out, and maybe reusable, but it’s also pretty context free. We’ve replaced the older `` with newer and more