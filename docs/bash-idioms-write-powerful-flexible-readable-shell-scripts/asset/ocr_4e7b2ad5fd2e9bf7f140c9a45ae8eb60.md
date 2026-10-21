![](bbox=[0.15, 0.08, 0.25, 0.17])

# Not RegEx

The pattern matching used in the case statement is not regular expressions. There is only one place in bash where regular expressions (regex or regexp) are allowed, and that’s in the if statement using the =~ comparison operator. If you really need to use regex, then you need to use a series of if/then/else statements instead of case.

# A Realistic Use Case

Code that parses command line options is a common place to find a case statement. Let’s look at a simple but somewhat realistic script that makes good use of a case statement.

# Motivation

If you’ve ever used Linux or Unix, you’ve likely made extensive use of the ls command to list out filenames and related information. Some options to ls are quite handy at giving more information or sorting it in certain ways. You might develop certain habits of how you use ls or the options you use most frequently. As a result, you might create some aliases or even entire scripts to make it easier to use your favorite combinations. But then you end up with several distinct scripts. They’re all related in functionality but all distinct. How might we combine them all into one script?

Consider a familiar example, the popular source control software called Git. It has several related but distinct functions, all invoked with the one command name git but each distinguished by a separate second keyword, for example, git clone, git add, git commit, git push, and so forth.

# Our Script

We can apply this “subcommand” approach to our situation. Let’s consider a few ls-related functions that we’d like to use: listing the files in order of filename length (along with the length), listing just the longest filename, listing the last few most recently modified files, and listing filenames with color coding indicating file type—a standard feature of ls but one that requires a few hard-to-remember options.

Our script will be named list, but there will be a second word to specify one of these functions. That word could be color, last, length, or long. Example 3-1 contains a script that does that.