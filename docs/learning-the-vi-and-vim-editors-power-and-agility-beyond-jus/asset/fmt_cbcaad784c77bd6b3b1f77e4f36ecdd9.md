
Vim’s `highlight` command lets you manipulate different groups and control how they are highlighted in your editing session. This command is powerful. You can inspect settings for various groups either as a list or by requesting specific group highlight information. For example:

```
:highlight comment
```

returns [Figure 11-22](#ch11.html#vi8-ch-11-fig-22). The first field lists the highlight name (in this example, “Comment”). The second field always displays the string “`xxx`” as it appears as defined by the highlight definitions in the terminal or GUI.

![](assets/lvv8_1122.png)

The output shows how comments in this file will appear. The `xxx` is dark gray on the printed page, but on the screen it’s red.^([9](#ch11.html#idm46601075601696)) The `term=bold` output means that on a terminal incapable of color, `Comments` will be shown in bold. `ctermfg=12` means that on a color terminal, such as an `xterm` on a color monitor, the foreground color for `Comments` will be the matching DOS color blue. Finally, `guifg=Red` means the GUI interface will display `Comments` with the foreground color red.

#### Note
The DOS color scheme is a more restricted set of colors than in modern GUI sets. In this scheme, there are only eight colors: `black`, `red`, `green`, `yellow`, `blue`, `magenta`, `cyan`, and `white`. Each of them can be set for text foreground or background and optionally can be defined as “bright,” a brighter color on the screen. Vim uses analogous mappings for defining text colors in non-GUI windows, e.g., in `xterm`s.

GUI windows offer virtually unlimited color definitions. Vim lets you define some colors with common names such as `Blue`, but you can also define these colors with red, green, and blue values. The format is `#rrggbb` where the `#` is literal, and `rr`, `gg`, and `bb` are hexadecimal numbers representing the level of each color. For example, red could be defined with `#ff0000`.

Use the `highlight` command to change settings for groups whose colors you don’t like. For example, we can find that identifiers in this file are dark cyan for our GUI interface, as shown in the output in [Figure 11-23](#ch11.html#vi8-ch-11-fig-23):

```
:highlight identifier
```

![](assets/lvv8_1123.png)

We can redefine the color for identifiers with the command:

```
:highlight identifiers guifg=red
```

Now all identifiers on the screen are (a rather ugly) red. This kind of customization is fairly inflexible: it applies to all file types and does not adapt to different backgrounds or color schemes.

To see how many highlight definitions exist and what their values are, again use `highlight`:

```
:highlight
```

[Figure 11-24](#ch11.html#vi8-ch-11-fig-24) shows a small sample of the results from the `highlight` command.

![](assets/lvv8_1124.png)

Note how some lines contain full definitions (listing `term`, `ctermfg`, and so on), whereas others receive their attributes from parent groups (e.g., `String` links back to `Constant`).

### Overriding syntax files
In the previous section, we learned how to define syntax group attributes for all instances of a group. Suppose you want to change a group for only one or a few syntax definitions. Vim lets you do this with the `after` directory. This is a directory in which you can create any number of `after` syntax files that Vim will execute after the normal syntax file.

To do this, simply include highlight commands (or any processing commands—the notion of “after” processing is generic) in the specific file in a directory named `after` that is included in the `runtimepath` option. Now when Vim sets up syntax highlighting rules for your file type, it will also execute your custom commands in the `after` file.

For example, let’s apply a customization to XML files, which use the `xml` syntax. This means Vim loaded syntax definitions from a file in the syntax directory named `xml.vim`. As in the previous example, we want to define identifiers always to be red. So we create our own file named `xml.vim` in a directory named `~/.vim/after/syntax`. In our `xml.vim` file we put the following line:

```
highlight identifier ctermfg=red guifg=red
```

Before this customization works, we must ensure that `~/.vim/after/syntax` is in the `runtimepath` path:

```
:set runtimepath+=~/.vim/after/syntax      In our .vimrc file
```

To make the change permanent, of course, the line should go in our `.vimrc` file.

Now, whenever Vim loads syntax definitions for `xml`, it will override the definitions for `identifier` with our own customization.

## Rolling Your Own
With the building blocks from the previous sections, we now have enough knowledge to write our own syntax files, simple as they might be. There are still many facets to learn before we can fully develop a syntax file.

We will incrementally build our own syntax file. Because syntax definitions can be extremely complex, let’s consider something simple enough to be easily grasped, but complex enough to show its potential power.

Consider an excerpt from a generated Latin file, `loremipsum.latin`:

```
Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Proin eget
tellus. Suspendisse ac magna at elit pulvinar aliquam. Pellentesque
iaculis augue sit amet massa. Aliquam erat volutpat. Donec et dui at
massa aliquet molestie. Ut vel augue id tellus hendrerit porta. Quisque
condimentum tempor arcu. Aenean pretium suscipit felis. Curabitur semper
eleifend lectus. Praesent vitae sapien. Ut ornare tempus mauris. Quisque
ornare sapien congue tortor.

In dui. Nam adipiscing ligula at lorem. Vestibulum gravida ipsum iaculis
justo. Integer a ipsum ac est cursus gravida. Etiam eu turpis. Nam laoreet
ligula mollis diam. In aliquam semper nisi. Nunc tristique tellus eu
erat. Ut purus. Nulla venenatis pede ac erat.

...
```

You create a new syntax file by creating a new file of that syntax name, in this case `latin`. Its corresponding Vim file is `latin.vim`, which you can create in your personal Vim runtime directory, `$HOME/.vim/syntax`. Then start your syntax definition simply by creating some keywords with the `syntax keyword` command. Choosing `lorem`, `dolor`, `nulla`, and `lectus` as keywords, you can inaugurate the syntax file with the line:

```
syntax keyword identifier lorem dolor nulla lectus
```

There still isn’t any syntax highlighting when you edit `loremipsum.latin`. More work needs to be done before highlighting is automatic. But for the time being, activate the syntax with the command:

```
:set syntax=latin
```

The text should now look something like [Figure 11-25](#ch11.html#vi8-ch-11-fig-25).

![](assets/lvv8_1125.png)

On the screen, the text words are black with the keywords in red. In the printed book, it’s harder to distinguish; the keywords are dark gray instead of black.

You may have noticed that the first occurrence of `Lorem` isn’t highlighted. By default, syntax keywords are case sensitive. Add the following line at the top of the syntax file:

```
:syntax case ignore
```

and you should now see `Lorem` included as a highlighted keyword.

Before we try this again, let’s make it all work automatically. After Vim tries to detect any file type, it optionally checks for other definitions, or even overriding definitions (which are not recommended), in a directory named `ftdetect` in your `runtimepath`. Therefore, create that directory under `$HOME/.vim` and create a file in it named `latin.vim` containing this single line:

```
au BufRead,BufNewFile *.latin set filetype=latin
```
