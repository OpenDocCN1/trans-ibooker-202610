+   1. Import `assert_cmd::Command`.
+   2. Create a `Command` to run `hello` in the current crate. This returns a `Result` which I assume is safe to `Result::unwrap` because the binary is found.
+   3. Execute the program and use `Assert::success` to “ensure the command succeeded.”

> **NOTE**
> I'll have more to say about the `Result` type in following chapters. For now, just know that this is a way to model something that could succeed or fail for which there are two possible variants, `Ok` and `Err`, respectively.

# Understanding Program Exit Values

What does it mean for a program to exit successfully? Systems programs should report a final exit status to the operating system. The Portable Operating System Interface (POSIX) standards dictate that the standard exit code is 0 to indicate success (think zero errors) and any number from 1 to 255 otherwise. I can show you this using the `bash` shell and the `true` command. Here is the manual page from `man true` for the version that exists on macOS:

```
TRUE(1)                                 BSD General Commands Manual
TRUE(1)

NAME
    true -- Return true value.

SYNOPSIS
    true

DESCRIPTION
    The true utility always returns with exit code zero.
```