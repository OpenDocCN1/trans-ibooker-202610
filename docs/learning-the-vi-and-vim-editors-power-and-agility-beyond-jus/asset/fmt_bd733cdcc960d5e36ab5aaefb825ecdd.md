
| `CTRL-W SHIFT-W` || Move to the next window above or to the left. This is the upward counterpart to the `CTRL-W W` command (note the difference in case). |

| `CTRL-W T`  
`CTRL-W CTRL-T` || Move to the top leftmost window. |

| `CTRL-W B`  
`CTRL-W CTRL-B` || Move to the bottom rightmost window. |

| `CTRL-W P`  
`CTRL-W CTRL-P` || Move to the previous (last accessed) window. |

# Moving Windows Around

You can move windows two ways in Vim. One way simply swaps the windows on the screen. The other way changes the actual window layouts. In the first case, window sizes remain constant while windows change position on the screen. In the second case, windows not only move but are resized to fill the position to which they’ve moved.

## Moving Windows (Rotate or Exchange)

Three commands move windows without modifying layout. Two of these rotate the windows positionally in one direction (to the right or down) or the other (to the left or up), and the other one exchanges the position of two possibly nonadjacent windows. These commands operate *only* on the row or column in which the current window lives.

`CTRL-W R` rotates windows to the right or down. Its complement is `CTRL-W SHIFT-R`, which rotates windows in the opposite direction.

An easier way to imagine how these work is to think of a row or column of Vim windows as a one-dimensional array. `CTRL-W R` shifts each element of the array one position to the right and moves the last element into the vacated first position. `CTRL-W SHIFT-R` simply moves the elements in the other direction.

If there are no windows in a column or row that aligns with the current window, this command does nothing.

After Vim rotates the windows, the cursor remains in the window from which the rotate command executed; thus, the cursor moves with the window.

`CTRL-W X` and `CTRL-W CTRL-X` let you exchange two windows in a row or column of windows. By default, Vim exchanges the current window with the next window, and if there is no next window, Vim tries to exchange with the previous window. You can exchange with the `n`th next window by prepending a count before the command. For example, to switch the current window with the third next window, use the command `3 CTRL-W X`.

As with the two previous commands, the cursor stays in the window from which the exchange command executes.

## Moving Windows and Changing Their Layout

Five commands move and re-layout the windows: two move the current window to a full-width top or bottom window, two move the current window to a full-height left or right window, and the fifth moves the current window to another existing tab. See the section ["Tabbed Editing"](#ch10.html#vi8-ch-10-sect-8) for information on tabbed editing. The first four commands bear familiar mnemonic relationships to other Vim commands; for instance, `CTRL-W SHIFT-K` maps to the traditional notion of `k` as "up." [Table 10-3](#ch10.html#vi8-ch-10-tab-3) summarizes these commands.^([2](#ch10.html#idm46601077375824))

Table 10-3. Commands to move and re-layout windows

| Command || Description |
| --- | --- |
| `CTRL-W SHIFT-K` || Move the current window to the top of the screen, using the full width of the screen. |
| --- | --- |
| `CTRL-W SHIFT-J` || Move the current window to the bottom of the screen, using the full width of the screen. |
| `CTRL-W SHIFT-H` || Move the current window to the left of the screen, using the full height of the screen. |
| `CTRL-W SHIFT-L` || Move the current window to the right of the screen, using the full height of the screen. |
| `CTRL-W SHIFT-T` || Move the current window to a new tab. |

It is difficult to describe the exact behavior of these layout commands. After the move and expansion of the window to the full height or width of the screen, Vim redoes the window layout in a reasonable way. The behavior of the layout can also be influenced by some of the windows’ options settings.

## Window Move Commands: Synopsis

Tables [10-4](#ch10.html#vi8-ch-10-tab-4) and [10-5](#ch10.html#vi8-ch-10-tab-5) summarize the commands introduced in this section.

Table 10-4. Commands to rotate window positions

| Command || Description |
| --- | --- |
| `CTRL-W R`  
`CTRL-W CTRL-R` || Rotate windows down or to the right. |
| --- | --- |
| `CTRL-W SHIFT-R` || Rotate windows up or to the left. |
| `CTRL-W X`  
`CTRL-W CTRL-X` || Swap positions with the next window, or if issued with a count `n`, swap with `n`th next window. |

Table 10-5. Commands to change position and layout

| Command || Description |
| --- | --- |
| `CTRL-W SHIFT-K` || Move the current window to the top of the screen and use the full width. The cursor stays with the moved window. |
| --- | --- |
| `CTRL-W SHIFT-J` || Move the current window to the bottom of the screen and use the full width. The cursor stays with the moved window. |
| `CTRL-W SHIFT-H` || Move the current window to the left of the screen and use the full height. The cursor stays with the moved window. |
| `CTRL-W SHIFT-L` || Move the current window to the right of the screen and use the full height. The cursor stays with the moved window. |
| `CTRL-W SHIFT-T` || Move the current window to a new tab. The cursor stays with the moved window. If the current window is the only window in the current tab, no action is taken. |

# Resizing Windows

Now that you’re more familiar with Vim’s multiwindowing features, you need a little more control over them. This section addresses how you can change the size of the current window, with, of course, effects on other windows in the screen. Vim provides options to control window sizes and window sizing behavior when opening new windows with split commands.

If you’d rather control window sizes *sans* commands, use `gvim` and let the mouse do the work for you. Simply click and drag window boundaries with the mouse to resize. For vertically separated windows, click the mouse on the vertical separator of pipe (`|`) characters. Horizontal windows are separated by their status lines.

## Window Resize Commands

As you’d expect, Vim has vertical and horizontal resize commands. Like the other window commands, these all begin with `CTRL-W` and map nicely to mnemonic devices, making them easy to learn and remember.

`CTRL-W =` tries to resize all windows to equal size. This is also influenced by the current values of `winheight` and `winwidth`, discussed in the following section. If the available screen real estate doesn’t divide equally, Vim sizes the windows to be as close to equal as possible.

`CTRL-W -` decreases the current window height by one line. Vim also has an `ex` command that lets you decrease the window size explicitly. For example, the command `:resize -4` decreases the current window by four lines and gives those lines to the window below it.

### Note

There is another mechanism that changes window size: the `lines` option. Normally Vim manages the `lines` value, and you can see (and) use the value by referencing `lines` with the `ex` command `set`:

```
:set lines
```
