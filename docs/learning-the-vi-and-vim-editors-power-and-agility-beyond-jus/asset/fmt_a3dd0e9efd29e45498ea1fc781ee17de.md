
Search forward or backward for `pattern`, and find the next match in the same direction, or in the opposite direction.

`v`

: 

Open the current file in the editor named by the `EDITOR` environment variable. This works only for a file, not for standard input.

## Configuring less’s display

A somewhat obscure feature of `less` is the ability to configure how it displays text. For most uses this isn’t interesting, but now that we’ve exchanged `less` for `more`, a real difference and a nice enhancement changes how you’ll see manual pages by issuing the following commands. Try viewing a few manual pages before and after to appreciate the visual effect (e.g., `man man`):

```
# variables and dynamic settings to improve less
export LESS="-ces -r -i -a -PM"
# Green:
export LESS_TERMCAP_mb=$(tput bold; tput setaf 2)
# Cyan:
export LESS_TERMCAP_md=$(tput bold; tput setaf 6)
export LESS_TERMCAP_me=$(tput sgr0)
# Yellow on blue:
export LESS_TERMCAP_so=$(tput bold; tput setaf 3; tput setab 4)
export LESS_TERMCAP_se=$(tput rmso; tput sgr0)
# White:
export LESS_TERMCAP_us=$(tput smul; tput bold; tput setaf 7)
export LESS_TERMCAP_ue=$(tput rmul; tput sgr0)
export LESS_TERMCAP_mr=$(tput rev)
export LESS_TERMCAP_mh=$(tput dim)
export LESS_TERMCAP_ZN=$(tput ssubm)
export LESS_TERMCAP_ZV=$(tput rsubm)
export LESS_TERMCAP_ZO=$(tput ssupm)
export LESS_TERMCAP_ZW=$(tput rsupm)
```

This file is available in the book’s [GitHub repository](https://www.github.com/learning-vi/vi-files). See the section [“Accessing the Files”](#app03.html#vi8-app-c-sect-1).

There are many more features. We leave the rest for you to explore. Hint: At the `less` prompt, type `h` to get help.

## screen

`screen` is a terminal session multiplexer providing multiple concurrent working sessions inside one terminal window. With terminal emulators available today with multiple sessions, usually implemented across tabs, what makes `screen` worthwhile? We will answer that question after presenting how to use `screen`, and in particular after showing a useful sample configuration file. So patience, please.

`screen` provides faithful terminal behavior for its sessions and thus requires you to use a special prefix character to activate its commands and functions. The default is CTRL-A. For example, to display a summary of most available `screen` commands, the `screen` command is `?`. So you must type CTRL-A ? to display the summary. Keep this in mind when experimenting with `screen`.

### Note

The following presentation assumes the use of a GNU/Linux or Unix system and that the `screen` application is available. Verify your system has `screen` with the `type` command from the shell prompt:

```
$ type screen
```

A response like `screen is /usr/bin/screen` is good. If you don’t get such a response, install `screen` with your package manager. Start at [*https://www.gnu.org/software/screen*](https://www.gnu.org/software/screen) if you want (or need) to build `screen` from source.

### Getting started with screen

`screen`, like many Unix applications, reads an optional user configuration file to set options and define sessions. The `screen` configuration file, `.screenrc`, lives in your home directory. To help follow the discussion, you can either copy the `.screenrc` file from the book’s [GitHub repository](https://www.github.com/learning-vi/vi-files) or create the file yourself and put the following lines in it:

```
startup_message off
defscrollback 20000

# Help screen, key bindings:
bindkey -k k9 exec sed -n '/^# Help/s/^/^M/p;/^# F[1-9]/p' $HOME/.screenrc
# F2 : list windows:
bindkey -k k2 windowlist
# F3 : detach screen (retains active sessions -- can reconnect later):
bindkey -k k3 detach
# F10: previous window (e.g., window 4 -> window 3):
bindkey -k k; prev
# F11: next window (e.g., window 2 -> window 3):
bindkey -k F1 next
# F12: kill all windows and quit screen (you will be prompted):
bindkey -k F2 quit

screen -t "edits for chapter 1"
screen -t "manage screen captures"
screen -t "manage todos"
screen -t "email and messages"
screen -t "git status and commits"
screen -t "system status"
screen -t "remote login to NAS" ssh 10.0.0.999
screen -t "solitaire"

select 1
```

Now start `screen`:

```
$ screen
```

You should see a normal-looking session, i.e., a command prompt in a window. The last line in the preceding code block, `select 1`, tells `screen` to start your interaction in the first window or session (defined as “edits for chapter 1”). The beauty of `screen` is that you are in a session, one of eight defined in the configuration file, each completely independent from the others.

`screen` has many ways to select sessions and navigate among them. We want to illustrate the `vi`-like navigation.

### Tip

`screen` uses single-character commands for most actions. Each of these single-character actions must be preceded by `screen`’s leader command-character, which by default is CTRL-A.

### The screen menu

`screen`’s command to display a list of available sessions is the double-quote (`"`). So, keeping the preceding tip in mind, you display the session menu with the two keystrokes, CTRL-A ". When you do, you should see something like [Figure 16-9](#ch16.html#vi8-ch-16-fig-9).

![](assets/lvv8_1609.png)

While it’s only very basic, you can select a session line with the standard line-up and line-down `vi` commands, `k`, and `j`. That’s not a lot of `vi`, but obviously in this context anything else is not really necessary. The point is that this is a philosophy of movement, and the `screen` developers chose `vi`.

### Navigating your session’s output

There is much more `vi` interaction in the `screen` sessions themselves. `screen` buffers text during your session(s), and you can navigate through this saved text using `vi` commands by typing CTRL-A ESC. By default, `screen` stores only about 100 lines of text. As you can see in our configuration above, we changed that default to 20,000. This is much more reasonable on modern systems. Really, make sure you set this. One hundred cached lines of text isn’t much at all.

Each buffer is maintained separately per session. Thus, in addition to command-line history that you can search, edit, and re-execute, you have `vi`-like access to both your commands *and* your commands’ outputs!

The basics for searching a `screen` buffer start with `vi`-basics. After typing CTRL-A ESC. you can search the buffer with motion commands (line up is `k`, line down is `j`, scroll up is `^B`, scroll down is `^F`, etc.). Forward search, as you might expect, is initiated with `/`, and backward search is initiated with `?`. Note that using `?` for your first search from the bottom of a buffer as a forward search will find nothing, since you are at the bottom already. To get more complete `vi`-like instructions for `screen`, go to `screen`’s man page and search for “vi-like.” See [Figure 16-10](#ch16.html#vi8-ch-16-fig-10).

![](assets/lvv8_1610.png)

With `screen` and its `vi`-like buffer navigation, you have access to information you’d probably thought you’d lost. That’s been our experience more than once.

### Take advantage of screen’s key bindings

In the sample `.screenrc` code shown earlier, there are lines unrelated to defining terminal sessions. These are our recommended key bindings. For example, the lines:

```
# F2 : list windows
bindkey -k k2 windowlist
```
