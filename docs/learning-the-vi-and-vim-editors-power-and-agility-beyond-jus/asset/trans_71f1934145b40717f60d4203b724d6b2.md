
Folds correspond to the semantics of a file’s language (e.g., a C program’s function blocks could fold).

You use these terms as the value of the `foldmethod` option. The manipulation of folds (opening and closing, deleting, etc.) is the same for all methods. We’ll examine manual folds and discuss Vim fold commands in detail. We address some details for the other methods, but they are complex, specialized, and beyond the scope of this introduction. We hope our coverage will prompt you to explore the richness of these other methods.

So let’s take a brief look at the important fold commands and go through a short example of what folds look like.

## The Fold Commands

Fold commands all begin with `z`. As a mnemonic to remember this, think of the side view of a folded piece of paper (when folded correctly) and how it looks like the letter “z.”

There are about 20 `z` fold commands. With these commands you create and delete folds, open and close folds (hide and expose text belonging to folds), and toggle the expose/hide state of the folds. Here are short descriptions:^([3](#ch11.html#idm46601076728064))

`zA`

Toggle the state of folds, recursively.

`zC`

Close folds, recursively.

`zD`

Delete folds, recursively.

`zE`

Delete *all* folds.

`zf`

Create a fold from the current line to the one where the following motion command takes the cursor.

`[count] zF`

Create a fold covering `count` lines, starting with the current line.

`zM`

Set option `foldlevel` to zero.

`zN`, `zn`

Set (`zN`) or reset (`zn`) the `foldenable` option.

`zO`

Open folds, recursively.

`za`

Toggle the state of one fold.

`zc`

Close one fold.

`zd`

Delete one fold.

`zi`

Toggle the value of the `foldenable` option.

`zj`, `zk`

Move the cursor to the start (`zj`) of the next fold or to the end (`zk`) of the previous fold. Note the mnemonic of the `j` and `k` motion commands and how they are analogous to motions within the context of folds.

`zm`, `zr`

Decrement (`zm`) or increment (`zr`) the value of the `foldlevel` option by one.

`zo`

Open one fold.

### Warning

Don’t confuse *deleting a fold* with *deleting text*. Use the “delete fold” (`zd`) command to remove, or undefine, a fold. A deleted fold has no effect on the text contained in that fold. You may notice we mention this more than once. It’s important to know. One of us has lost work thinking he merely deleted folds when, in fact, he deleted content. Of course this is something that is always discovered inconveniently late.

`zA`, `zC`, `zD`, and `zO` are called recursive because they operate on all folds nested within the one where you issue the commands.

## Manual Folding

If you know Vim motion commands, you already know half of what you must learn to be proficient with manual fold commands.

For example, to hide three lines in a fold, enter either of the following:

```
3zF
2zfj
```

`3zF` executes the `zF` folding command over three lines, starting with the current one. `2zfj` executes the `zf` folding command from the current line to the line where `j` moves the cursor (two lines down).

Let’s try a more sophisticated command of use to C programmers. To fold a block of C code, position the cursor over the beginning or ending brace (`{` or `}`) of a block of code and type `zf%`. (Remember that `%` moves to the matching brace.)

Create a fold from the cursor to the beginning of the file by typing `zfgg`. (`gg` goes to the beginning of the file.)

It is easier to understand folds by seeing an example. We’ll take a simple file, create and manipulate folds, and watch the behavior. We’ll also see some of the enhanced visual folding cues that Vim provides.

First consider the example file in [Figure 11-2](#ch11.html#vi8-ch-11-fig-2), which contains some (meaningless) lines of C code. Initially, there are no folds.

![](assets/lvv8_1102.png)

There are a few things to note in this picture. First, Vim displays line numbers on the left side of the screen. We recommend that you always turn them on (using the `number` option) for added visual information about file location, and in this context the information becomes more valuable when you fold lines out of view. Vim tells you how many lines are not displayed, and the line numbers confirm and reinforce that information.

Also notice the gray columns to the left of the line numbers. These columns are reserved for more folding visual cues. As we create and use folds, we will see the visual cues that Vim inserts into these columns.

In [Figure 11-2](#ch11.html#vi8-ch-11-fig-2), notice that the cursor is on line 18. Let’s fold that line and the two following lines into one fold. We type `zf2j`. [Figure 11-3](#ch11.html#vi8-ch-11-fig-3) shows the result.

![](assets/lvv8_1103.png)

Notice how Vim creates an easily identified marker with the `+--` as a prefix, and how it displays text from the first folded line in the *fold* placeholder. Now notice where Vim inserted the `+` at the far left side of the screen. This is another visual cue.

In the same file, we’ll next fold the block of code between and including the braces after the `if` statement. Position the cursor on either one of the braces and type `zf%`.^([4](#ch11.html#idm46601076653168)) The file now appears as in [Figure 11-4](#ch11.html#vi8-ch-11-fig-4).

![](assets/lvv8_1104.png)

Now there are eight lines of code folded, three of which are contained in a previously created fold. This is called a *nested* fold. Note that there is no indication of the nested fold.

Our next experiment is to position the cursor on line 25 and fold all lines up to and including the function declaration for `fcn`. This time we use the Vim *search* motion. We initiate the fold command with `zf`, search backward to the beginning of the `fcn` function using `?int fcn` (the backward search command in Vim), and press the ENTER key. The screen now looks like [Figure 11-5](#ch11.html#vi8-ch-11-fig-5).

![](assets/lvv8_1105.png)

### Note

If you count lines and create a fold that spans another fold (for example, `3zf`), all lines contained in the spanned fold count as one line. For example, if the cursor is on line 30, and lines 31–35 are hidden in a fold on the next screen line so that the next line on the screen displays line 36, `3zf` creates a new fold containing three lines as they appear on the screen: the text line 30, the five lines contained in the fold holding lines 31–35, and the text line 36 displayed in the next line on the screen. Confusing? A little. You might say that the `zf` command counts lines in accordance with the rule “What you see is what you fold.”

Let’s try some other features. First, open all the folds with the command `zO` (that’s `z` followed by the letter O, not `z` followed by a zero). Now we start seeing some visual cues in the left margin about the folds we made, as shown in [Figure 11-6](#ch11.html#vi8-ch-11-fig-6). Each of the columns in this margin is called a fold column.

![](assets/lvv8_1106.png)

In this figure, the first line of each fold is marked with a minus sign (`–`), and all the other lines of the fold are marked by a vertical bar or pipe character (`|`). The largest (outermost) fold is in the leftmost column, and the innermost fold is in the rightmost column. As you see in our picture, lines 5–25 represent the lowest fold level (in this case, one), lines 15–22 represent the next fold level (two), and lines 18–20 represent the highest fold level.

### Tip
