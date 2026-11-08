
# Vim Commands

### Scrolling and Redrawing
| `CTRL-E`, `CTRL-Y` || Show one more line at the bottom, top of the screen. If possible, Vim maintains the cursor position on the same line—e.g., if the cursor is on line 50 and you use this movement, the cursor moves up or down to stay on line 50. |
| `z ENTER` || Reposition the line with the cursor to the top of the screen. |
| `z.` || Reposition the line with the cursor to the middle of the screen. |
| `z-` || Reposition the line with the cursor to the bottom of the screen. |
| `CTRL-L` || Redraw the screen (without scrolling). |
| `CTRL-R` || `vi`: redraw the screen (without scrolling). |
|  || Vim: redo the last undone change. |

### Within a Screen
| `H` || Move to home—the first character of the top line on the screen. |
| --- | --- |
| `M` || Move to the first character of the middle line on the screen. |
| `L` || Move to the first character of the last line on the screen. |
| `n` `H` || Move to the first character of the line `n` lines below the top line. |
| `n` `L` || Move to the first character of the line `n` lines above the last line. |

### Searches
| `/pattern` || Search forward for `pattern`. End with `ENTER`. |
| --- | --- |
| `/pattern/+ num` || Go to line `num` after `pattern`. Forward search for `pattern`. |
| `/pattern/- num` || Go to line `num` before `pattern`. Forward search for `pattern`. |
| `?pattern` || Search backward for `pattern`. End with `ENTER`. |
| `?pattern?+ num` || Go to line `num` after `pattern`. Backward search for `pattern`. |
| `?pattern?- num` || Go to line `num` before `pattern`. Backward search for `pattern`. |
| `:noh` || Suspend search highlighting until next search. {Vim} |
| `n` || Repeat the previous search. |
| `N` || Repeat the previous search in the opposite direction. |
| `/` || Repeat the previous search forward. End with `ENTER`. |
| `?` || Repeat the previous search backward. End with `ENTER`. |
| `*` || Search forward for the word under the cursor. Matches only exact words. {Vim} |
| `#` || Search backward for the word under the cursor. Matches only exact words. {Vim} |
| `g*` || Search forward for the word under the cursor. Matches the characters of this word when embedded in a longer word. {Vim} |
| `g#` || Search backward for the word under the cursor. Matches the characters of this word when embedded in a longer word. {Vim} |
| `%` || Find match of current parenthesis, brace, or bracket. |
| `f x` || Move the cursor forward to `x` on current line. |
| `F x` || Move the cursor backward to `x` on current line. |
| `t x` || Move the cursor forward to the character before `x` in current line. |
| `T x` || Move the cursor backward to the character after `x` in current line. |
| `,` || Reverse the search direction of the last `f`, `F`, `t`, or `T`. |
| `;` || Repeat the last `f`, `F`, `t`, or `T`. |

### Line numbering
| `CTRL-G` || Display the current line number. |
| --- | --- |
| `gg` || Move to the first line in the file. {Vim} |
| `num G` || Move to line number `num`. |
| `G` || Move to the last line in the file. |
| `: num` || Move to line number `num`. |

### Marks
| `m x` || Place mark `x` at the current position. |
| --- | --- |
| `` `x `` || (Backquote.) Move cursor to mark `x`. |
| `'x` || (Apostrophe.) Move to the start of the line containing `x`. |
