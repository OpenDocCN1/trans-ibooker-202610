# Unofficial bash Strict Mode

Perl has a use strict pragma to “restrict unsafe constructs” that causes a syntax check or run time error if you do not declare and initialize variables before using them, among other things. The unofficial bash strict mode article makes the argument that the following does the same (in spirit) in bash:

```
set -euo pipefail
IFS=$'\n\t'
```

We don’t quite agree on changing $IFS like that because that will break a lot of bash idioms (see “Fiddling with $IFS for Fun and Profit, to Read Files” on page 89). The author makes a good argument, but it might be a large effort to migrate legacy code for that. The set commands are great, though:

+   - -e will exit the entire script at the first error.
+   - -u will exit the entire script if you use an unset variable.
+   - -o pipefail will fail pipelines if any command, not just the last one, exits with nonzero.

Adding -u will break a lot of existing scripts, but the fixes will be pretty easy, and you may find some variable name typos you didn’t know you had, which is the whole point. -eo pipefail will probably break less up front, and when that does “break” a script, it was probably already broken, but the error was masked and these options just unmasked the problem.

This is definitely worth considering and perhaps adding to your templates and style guide.

See also:

+   - http://redsymbol.net/articles/unofficial-bash-strict-mode
+   - help set
+   - https://perldoc.perl.org/strict

# Exit Codes

Unless told otherwise, your bash script will exit with the exit (or return) code of the last command run. Sometimes that’s fine, and other times you may need to exit earlier after failing a sanity check or for some other condition.