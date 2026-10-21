
# ch02.asciidoc:=== Finding data
egrep '^===' ch*.asciidoc | egrep -v '^ch00.asciidoc' \
> $BOOK_ASC/index.txt && {
    echo "Updated: $BOOK_ASC/index.txt"
    exit 0
} || {
    echo "FAILED to update: $BOOK_ASC/index.txt"
    exit 1
}
;;

rerun )            ## Run examples to re-create (existing!) output files
    # Only re-run for code that ALREADY HAS a *.out file...not ALL *.sh code
    for output in examples/*/*.out; do
        code=${output/out/sh}
        echo "Re-running code for: $code > $output"
        $code > $output
    done
;;

cleanup )           ## Clean up all the xHTML/XML/PDF cruft
    rm -fv {ch??,app?}.{pdf,xml,html} book.{xml,html} docbook-xsl.css
;;

* )
    \cd -  # UGLY cheat to revert the 'cd' above...
    ( echo "Usage:"
    egrep '\()[[:space:]]+#+ '  $0
    echo ''
    egrep '\()[[:space:]]+## ' $0
    echo ''
    egrep '\()[[:space:]]+### ' $0 ) | grep "$S{1:-.}" | more
;;
esac

We’ve got a lot going on in there, so let’s unpack it all:

+   1. The real script does a lot of operations on the AsciiDoc source code for this book, so it’s just easier to make sure we’re in the right place and that we have a handy environment variable set.

+   2. We usually use $PROGRAM to hold a bash basename, but in this case we’re going to be calling this script recursively a lot, so $SELF just seemed more intuitive.

+   3. As we’ll discuss more in Chapter 11, using meaningful variable names instead of positional arguments is a good idea, so let’s do that.

4 And once we’ve captured the action, we don’t need the old $1 anymore, but there might be more options, so shift $1 away.

5 If /usr/bin/xsel exists and is executable, and if there are no more options, we know we’re reading and writing from the X Window clipboard, otherwise we’re getting text from the arguments and sending output to STDOUT. In practice, we copy from an editor, switch to the command line, run the tool, switch back, and paste.

6 This is where we start actually doing something—that is, figuring out what our “action” is.

7 For code organization and readability, break actions up into sections; see also 11.

8 Let’s start with markup for headers.

9 This line is both code and documentation. The $action is that we want a top-level (for the book code) header, h1. We’ll see later how that is also documentation.

10 Do the work. First, call ourselves to get an AsciiDoc “ID” for the text, then output that ID in double square brackets, followed by a newline, then have the text indented with === for the header level, then, finally, call the Output function. Hopefully the rest of the code is easy to understand.

11 For code organization and readability, break actions up into sections; see also 7.

12 You can break long lines with a trailing \; see also “Layout” on page 130.

13 Things get interesting again in the catch-all, which combines help or usage with unknown argument handing and a great “search in help” feature.

14 We wrap whatever output we have into a subshell we pipe into more in case it’s long.

15 Here’s the “line of code as documentation” we talked about in callout 9. We grep for the closing paren, ), from our case statement, followed by spaces, followed by a single comment marker, #. That gives us our level 1 “Content/Markup” actions. That pulls out the actual lines of code that make the case statement work, but it also shows you what it does because of how we’ve added the comments.

16 This does the same for the level 2 “Tools” section.

17 This would do the same for level 3, which handles Git operations, but we’ve omitted that code for simplicity here. But it also uses grep with ${1:-.} to show us help on either something we asked for, like wrapper.sh help heading, or everything (grep "."). With a script this short, that may not seem like a big deal, but when it grows over time (and it will), that becomes really handy!

The result of the grep commands and the “levels” noted previously is to display a help message that is sorted but grouped into “level 1” and “level 2” sections:

```
$ examples/ch03/wrapper.sh help
Usage:
    h1 )                # Inside chapter heading 1 (really AsciiDoc h3)
    h2 )                # Inside chapter heading 2 (really AsciiDoc h4)
    h3 )                # Inside chapter heading 3 (really AsciiDoc h5)
    bul|bullet )        # Bullet list (** = level 2, + = multiline element)
    nul|number|order* ) # Numbered/ordered list (.. = level 2, + = multiline)
    term )              # Terms
    bold )              # Inline bold (ORA prefers italics to bold)
    i|italic*|itl )     # Inline italics (ORA prefers italics to bold)
    c|constant|cons )   # Inline constant width (command, code, keywords, more)
    type|constantbold ) # Inline bold constant width (user types literally)
    var|constantitalic )# Inline italic constant width (user-supplied values)
    sub|subscript )     # Inline subscript
    sup|superscript )   # Inline superscript
    foot )              # Create a footnote
    url|link )          # Create a URL with alternate text
    esc|escape )        # Escape a character (esp. *)
    id )                ## Convert a hack/recipe name to an ID
    index )             ## Creates 'index.txt' in AsciiDoc dir
    rerun )             ## Run examples to re-create (existing!) output files
    cleanup )           ## Clean up all the xHTML/XML/PDF cruft
```

```
$ examples/ch03/wrapper.sh help heading
    h1 )                # Inside chapter heading 1 (really AsciiDoc h3)
    h2 )                # Inside chapter heading 2 (really AsciiDoc h4)
    h3 )                # Inside chapter heading 3 (really AsciiDoc h5)
```

## One More Twist

At the end of each bit of code associated with a pattern, we ended with a double semicolon. In our first example at the start of this chapter, we wrote:

```
"yes") echo "glad you agreed" ;;
```

After the echo command, we put a ;;, which indicates that no further action should be taken. Execution will continue after the esac keyword.

But sometimes you don’t want that behavior. In some situations, you might want other patterns in the case statement to be checked, or other actions taken. The syntax in bash allows for this, with ;;& and ;& used to indicate these variations.

Here’s an example of that behavior that provides details about the path in $filename:

```
case $filename in
  ./*) echo -n "local "          # Begins with ./
      ;&                           # Fall through!
  [^/]*) echo -n "relative "      # Starts w/ anything but a slash
      ;;&                          # Look for other matches
  /*) echo -n "absolute "         # Begins with a slash
      ;&                           # Fall through
  */*) echo "pathname"            # A slash anywhere
      ;;                           # Done
  *) echo "filename"              # All other
      ;;                           # Done
esac
```

The patterns will be compared, in order, to the value in $filename. The first pattern is two literal characters—a period and a slash—followed by any characters. If that matches (e.g., if the value of $filename was ./this/file), then the script will print “local” but without the newline at the end. The next line is ;&, which tells bash to “fall through” and execute the command(s) associated with the next pattern (without even checking for a match). So it will also print “relative.” Unlike the previous pattern, this section of code ends with ;;&, which tells bash to try other patterns (going forward, in order) for a match, too.

So now it will check the next pattern, looking for a leading slash. If that doesn’t match, the next one might. It looks for a slash anywhere in the string (any—zero or more—characters, then a slash, then any characters). If that matches (and it would in our example), it will print the word pathname. The ;; indicates that no more patterns need to be examined, and it would be done.

## Style and Readability: Recap

In this chapter, we described the case statement, an n-way branch in the flow of execution. Its pattern-matching feature makes it very useful in scripting, though a common use is a simple, literal match of particular words.

The variations ;;, ;;&, and ;& provide some useful functionality but can be tricky. It might be better to structure such logic using if/then/else rather than a case statement.

These symbols are so subtly different that it can be easy to overlook what happens at each step. The control flow after a match is made can be different for each case: to fall through executing more code, to try to match another pattern, or to be done. Therefore we strongly encourage you to comment on your choices in your scripts to avoid confusion or misunderstanding.
