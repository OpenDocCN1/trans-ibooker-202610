
For most users, simply undoing and redoing changes is sufficient. But consider a more complex scenario. What if you make seven changes to a file, and undo three? So far, so good, nothing unusual to consider. But now suppose that after undoing three out of seven changes, you then make a change different from the next forward change in Vim’s collection of changes; Vim defines that point in the change history as a *branch* from which different paths of changes occur. With that path you can now move back and forth chronologically, with the added twist that at a branch point you can move forward along any of the different paths of recorded changes.

For more complete descriptions of how to navigate changes as a tree, use Vim’s help command:

```
:help usr_32.txt
```

For deeper analyses of Vim change-trees, check out some of the `undo` plug-ins at [*https://vimawesome.com/?q=undo*](https://vimawesome.com/?q=undo).

# Incremental Searching

When *incremental searching* is used, the editor moves the cursor through the file, matching text *as you type* the search pattern. When you finally type ENTER, the search is finished.^([7](#ch08.html#idm46601078793360)) Vim enables incremental searching with the `incsearch` option. When enabled, Vim highlights the text that matches what you’ve typed so far, changing what it highlights as you enter more characters into the search pattern.

If you’ve never seen it before, it is rather disconcerting at first. However, after a while you get used to it, and eventually you come to wonder how you ever did without it. We strongly recommend adding `set incsearch` to your `.vimrc` file.

# Left-Right Scrolling

By default, `vi` and Vim wrap long lines around the screen. Thus, a single logical line of the file may occupy multiple physical lines on your screen.

There are times when it might be preferable for a long line to simply disappear off the righthand edge of the screen instead of wrapping. Moving onto that line and then moving to the right would “scroll” the screen sideways.

In Vim, a numeric option (`sidescroll`, default value zero) controls how much to scroll the screen, and a Boolean option (`wrap`, default true) controls whether lines wrap or disappear off the edge of the screen. Thus, to get side scrolling, you’d use `:set nowrap` and give `sidescroll` a reasonable value such 8 or 16.

# Summary

For many years `vi` was the standard text-editing tool on Unix. `vi` was almost revolutionary in its time, with its dual mode orientation and touch-edit philosophy. Vim continues where `vi` left off, and it is the next evolutionary step for powerful editing and text management:

* Vim extends `vi`, building on the excellent standard set by the older editor. Although other editors have also built upon the original, Vim has emerged as the most popular and widely used `vi` clone.
* Vim offers far (far!) more than `vi`, so much more as to become the new standard. Vim is the de facto standard in that most Unix-like operating systems link the `vi` command to Vim.
* Vim is for beginners *and* for power users. For beginners, it offers various learning tools and “easy” modes, whereas for experts it offers powerful extensions to `vi`, along with a platform on which power users can enhance and tune Vim to their exact needs.
* Vim runs everywhere. As discussed earlier, in environments in which Vim wasn’t available, others stepped in and ported it to most useful OS platforms. Vim may not literally be everywhere, but it’s close!
* Vim is free. Furthermore, Vim is charityware. The work Bram Moolenaar has done creating, improving, maintaining, and sustaining Vim is one of the truly remarkable feats in the free software market. If you like *his* work, Bram invites you to learn about his favorite cause, helping children in Uganda. More information is available at the website [ICCF Holland](http://iccf-holland.org), or you can simply use Vim’s built-in help command, topic “uganda” (`:help uganda`).

^([1](#ch08.html#idm46601079816288-marker)) We note that [Wikipedia](https://en.wikipedia.org/wiki/Vim_\(text_editor\)#History) cites the *original* attribution for Vim: “At the time of its first release, the name ‘Vim’ was an acronym for ‘Vi IMitation.’ ”

^([2](#ch08.html#idm46601079799872-marker)) Ease of use is subjective, but we strongly believe that users who invest the time to learn Vim’s extended features will agree with this assessment.

^([3](#ch08.html#idm46601079789024-marker)) This section is adapted from material supplied by Bram Moolenaar, Vim’s author. We thank him. You can find more information on Vim’s history in [Bram’s “Vim 25” talk](https://www.youtube.com/watch?v=ayc_qpB-93o).

^([4](#ch08.html#idm46601079768032-marker)) You can find these files from your Vim session. The help directory is `doc` under the `$VIMRUNTIME` directory. Type the `ex` command `:!ls $VIMRUNTIME/doc`.

^([5](#ch08.html#idm46601079723408-marker)) Well, up to the maximum value of a C `long`—2,147,483,647 on a 32-bit computer, considerably more on a 64-bit system.

^([6](#ch08.html#idm46601079003904-marker)) The `*`, `\+`, and `\=` operators can be reduced to `\{0,}`, `\{1,}`, and `\{0,1}`, respectively, but the former are much more convenient to use. Also, interval expressions were developed later in the history of Unix regular expressions.

^([7](#ch08.html#idm46601078793360-marker)) Emacs has always had incremental searching.

# Chapter 9. Graphical Vim (gvim)

As a `vi` derivative, Vim began as a project to extend `vi` by adding features not available in `vi`. As an independent effort Vim added and improved on the excellent `vi`, and Vim could do this quickly, based on user feedback, without the onus of POSIX requirements.

Already at the time of this book’s seventh edition, Vim offered mature and comprehensive graphical user interface (GUI) features discussed in this chapter. In the years since then, Vim continued enhancing the GUI, and today it is better than ever.

A longtime complaint about `vi` and its clones was their lack of a GUI. Especially for those caught up in the Emacs versus `vi` religious wars, `vi`’s lack of a GUI was the ultimate trump card to argue that `vi` was a nonstarter when discussing editors. That is a complaint long since answered.

The `vi` clones and “work-alikes” created their own GUI versions. Graphical Vim is called `gvim`. Like the other `vi` clones, `gvim` offers robust and extensible GUI functions and features. We’ll cover the most useful ones in this chapter.

Some of `gvim`’s graphical functionality wraps commonly used Vim features, whereas others introduce the point-and-click convenience most computer users now expect. Although some veteran Vim users may cringe at the thought of grafting a GUI onto their workhorse editor, `gvim` is thoughtfully conceived and implemented. `gvim` offers functionality and features spanning the range of its users’ abilities, softening Vim’s steep learning curve for beginners and transparently bringing expert users extra editing power. This strikes a nice compromise.

### Tip

`gvim` for MS-Windows comes with a menu entry labeled “easy `gvim`.” This is indeed valuable to people who have never used Vim, but ironically, it is anything *but* easy for expert users.

In this chapter we first discuss the general `gvim` GUI concepts and features, with a brief introductory section about mouse interaction. Additionally, we refine the discussion around differences and things you should know for different `gvim` environments. Specifically, we focus on MS-Windows and the X Window System, the two main graphical platforms.^([1](#ch09.html#idm46601078720784)) We also provide a brief list of GUI options with synopses.

## General Introduction to gvim
