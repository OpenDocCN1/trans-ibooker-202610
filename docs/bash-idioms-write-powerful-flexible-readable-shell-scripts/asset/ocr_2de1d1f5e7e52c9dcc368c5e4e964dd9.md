From the bash docs:

If the subscript is @ or *, the word expands to all members of the array name. These subscripts differ only when the word appears within double quotes. If the word is double-quoted, "${name[*]}" expands to a single word with the value of each array member separated by the first character of the IFS variable (see “Fiddling with $IFS for Fun and Profit, to Read Files” on page 89), whereas "${name[@]}" expands each element of name to a separate word.

That’s a mouthful isn’t it? We’ve already talked about that in “Quotes and Spaces” on page 14, but it’s important because, as we’ll show, if you get it wrong you can hurt yourself. We’ve used printf "%q" with a pipe (|) delimiter in Example 7-1 to show “shell quoted” strings (technically “words”) in a way that is visible in the output on the screen or on this page. The quoting rules are actually the same as we cover in Chapter 2, just in the context of a list or hash.

# Lists

As we said, arrays, also known as lists, are variables containing multiple elements that are indexed or referred to by an integer.

In bash they start from zero and may be declared using declare -a, local -a, readonly -a, or by just assigning to a new variable like mylist[0]=foo or mylist=() (empty list). Once a variable is declared as a list, a simple assignment like mylist+=(bar) is the same as a push, which adds an item to the end of the list, but note the + and (), both of which are critical. See Table 7-1 for an example.

Table 7-1. Sample bash list

| Element | Value |
|---|---|
| mylist[0] | foo |
| mylist[1] | bar |
| mylist[2] | baz |

The common operations on an array or list are:

+   - Declare a variable as a list
+   - Assign one or more values to it
+   - If treating it as a stack (think plates in a cafeteria; FIFO: first on, first off, or first in, first out):
+     - push
+     - pop
+   - Display (dump) all values for debugging or reuse