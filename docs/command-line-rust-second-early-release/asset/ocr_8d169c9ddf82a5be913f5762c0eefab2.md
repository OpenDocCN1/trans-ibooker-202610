+   - Implement the negative number options from the GNU version where -n==-3 means you should print all but the last three lines of the file. As always, create tests to ensure your program is correct.
+   - Add an option for selecting characters.
+   - Add the file with the Windows line endings to the tests in Chapter 3. Edit the `mk-outs.sh` for that program to incorporate this file, and then expand the tests and program to ensure that line endings are preserved.

# Summary

This chapter dove into some fairly sticky subjects such as converting types like a `&str` to a `usize`, a `String` to an `Error`, and a `usize` to a `u64`. I feel like it took me quite a while to understand the differences between `&str` and `String` and why I need to use `From::from` to create the `Err` part of `MyResult`. If you still feel confused, just know that you won’t always. I think if you keep reading the docs and writing more code, it will eventually come to you.

Here are some things you accomplished in this exercise:

+   - You learned to create optional parameters that can take values. Previously, the options were flags.
+   - You saw that all command-line arguments are strings. You used the `str::parse` method to attempt the conversion of a string like “3” into the number 3.
+   - You learned how to write and run a unit test for an individual function.
+   - You learned to convert types using the `as` keyword or with traits like `From` and `Into`.