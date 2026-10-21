As the bash reference says, “Bash provides one-dimensional indexed and associative array variables.” While it’s possible to create really ugly multidimensional structures, doing so in bash will probably end in tears. If you really need to do that, consider doing it in a different language.

![](bbox=[0.15, 0.2, 0.25, 0.27])

# Watch Out for bash Versions with Hashes

You really need to be careful about your bash version here. As we just said, bash didn’t get hashes (i.e., associative arrays) until version 4.0, and it took a couple of versions to sort out some details, like allowing $list[-1] to refer to the last integer element (v4.3), instead of needing $mylist[${#mylist[*]}-1] (where ${#mylist[*]} is the element count). Ouch.

As we said in “bash on Mac” on page ix, watch out for stock bash on a Mac; it’s quite old. You’ll find newer versions in MacPorts, Homebrew, or Fink.

![](bbox=[0.15, 0.4, 0.25, 0.47])

# Not POSIX

Also, arrays (lists and hashes) are not specified by POSIX, so if portability beyond bash is a concern, you’ll need to be very careful using them because they might not work. For example, the Zsh syntax is a bit different, so these examples won’t work on that on a Mac either.

# Commonalities

Lists and hashes are very similar in bash, so we’re going to start with what they have in common, then get to where they are different. In fact, you can treat lists as a subset of hashes that simply happen to have ordered integer indexes. We don’t think you should do that, but you can.

Lists are inherently ordered, whereas hashes are inherently unordered, so there are operations like shift or push that only make sense for an ordered list. On the other hand, you would never need to sort the keys in an ordered list, but you might for a hash.

![](bbox=[0.15, 0.76, 0.25, 0.83])

# Accidental Assignment

An accidental assignment without a subscript will affect element zero, so myarray=foo results in creating or overwriting $myarray[0] with foo, even if it’s a hash!