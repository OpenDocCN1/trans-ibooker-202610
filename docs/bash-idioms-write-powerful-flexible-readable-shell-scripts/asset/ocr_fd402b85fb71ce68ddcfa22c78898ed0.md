```
##################################################
# Please don't build boxes like this!            #
#                                                #
# They make it VERY painful to edit the comments later, because #
# now you have to worry about the closing character on the      #
# righthand side. This example isn't too bad, but it gets       #
# out of hand quickly.                           #
##################################################
```

Do this:

```
##################################################
# Please DO build boxes like this!
#
# This one is easier to edit, because while you still have to
# worry about text wrapping, it's just a simple leading "#" when
# you do rewrap.
```

# Names

Naming is *important*. We can’t stress that enough. The difference between `$file` and `$critical_process_daily_log_file` doesn’t seem like anything except some extra typing *now*, when all the details are in your head. But we guarantee if you take the extra time to really think about what you are naming and how it will read in the code, that will pay off soon, by reducing coding errors, and in the future when rereading, debugging, and enhancing. Following are style guidelines related to names:

+   - Good names are critical!
+   - Global variables and constants are in UPPER case.
+     - Prefer not to make changes to global variables, but sometimes that’s just much simpler (KISS).
+     - Use `readonly` or `declare -r` for constants.
+   - Other variables are in lowercase.
+   - Functions are in Mixed_Case.
+   - Use "_", not CamelCase, in place of space (remember, "-" is not allowed in variable names).
+   - Use bash arrays carefully; they can be hard to read (see Chapter 7). `for var in $regular_var` often works as well.
+   - Replace $1, $2, .. $N with readable names ASAP.
+     - That makes everything much more debuggable and readable, but it also makes it easy to have defaults and add or rearrange arguments.
+   - Distinguish between types of referents, like $input_file versus $input_dir.