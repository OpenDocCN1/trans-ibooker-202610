# Example 10-3. Example bash debug prompt

```
export PS4='+xtrace $BASH_SOURCE:$LINENO:$FUNCNAME: '
```

# PROMPT_COMMAND

Example 10-4 shows a command to run just before $PS1 is evaluated and displayed. It’s used for all kinds of things like updating the window title for GUI terminals; displaying dynamic details about your environment, like what Git branch you’re in; or even doing some very primitive and insecure logging.

# Example 10-4. Example bash PROMPT_COMMAND for logging

```
### This should be one line; it is broken to fit the page
export PROMPT_COMMAND='logger -p local1.notice -t "bashlog[$S]" \
"SSH=$SSH_CONNECTION; USER=$USER; PATH=$PWD; COMMAND=$(fc -ln -1)"'
```

# bash Time Zone

This isn’t really a bash idiom, but it’s a neat prompt hack. The bash prompt defaults to displaying the date and time in the system time zone, but you can export TZ=UTC or some other time zone to change that. That’s useful if you prefer to keep your GUI in your local time zone but would rather have bash display UTC for documentation purposes, or to have local bash time displays match server displays. Note that the prompt TZ will not change until after you run an external command; just hitting enter or running a bash builtin will not cause it to update.

See also “Time for printf” on page 55 for more date and time handling.

# Getting User Input

Most of the time, bash scripts get user input via command line arguments, but sometimes you might need to ask for input as well. The two common ways to do that are the read and select builtins.

See also:

+   - help read
+   - help select

# read

read reads a line from STDIN or a file descriptor and assigns the words to a list of variables, or into an array (list) variable, and it has a number of really useful options (shown in Example 10-5).