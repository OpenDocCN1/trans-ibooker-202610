In bash, zero means *success* and nonzero means failure. This is the opposite of many other languages, but it’s this way because there is only one way to succeed but many ways to fail, and you might need to represent which way you failed, like:

```
if grep --quiet "$HOSTNAME" /etc/hosts; then ...
```

The bash exit code is only eight bits, so the maximum value is 255, but you want your return code values to stay at or below 125 because 126 and above are taken, as shown in Table 9-1.

# Table 9-1. bash exit/return codes

| Exit/return code | Use/description |
|---|---|
| 0 | Success |
| 1–125 | User-defined failure |
| 2 | bash builtins incorrect usage |
| 126 | Command found but not executable |
| 127 | Command not found |
| 128–255 | Command exited with signal N-128 |

See also the GNU documentation about exit status.

The command to exit the entire script with an “exit code” is, wait for it, exit n. The command to return from a function is, again, wait for it, return n, where “n” is an optional argument as defined previously. Remember that return returns an exit code, not values. The “n” can be any shell expression like 5 or $val or something more complex. Remember if “n” is not given, status is that of the last command run.

See also our discussion about exit 0 or exit 1 for help and usage in “HELP!” on page 79.

# It’s a Trap!

The trap builtin (help trap) allows you to execute code when the script receives a signal, which includes when it exits. That is a great place to have “cleanup” code that will work even if someone CTRL-Cs (but not if they kill -9) the script. You can also use it to run debugging code, but for this simple case, just use set -x or run under bash -x. Run trap -l to list all the signals your system supports or trap -p <signal> to show the trap commands associated with that signal.

The typical use of trap is to clean up after your script, as noted. The syntax is trap [-lp] [[arg] signal_spec ...], and while that arg can be self-contained code, it’s probably cleaner to just define and then call a function instead. That looks something like Example 9-12 (output shown in Example 9-13).