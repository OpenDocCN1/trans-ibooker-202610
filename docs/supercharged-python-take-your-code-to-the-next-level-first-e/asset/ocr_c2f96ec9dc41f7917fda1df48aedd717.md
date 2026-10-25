# # 6. Regular Expressions, Part I

Increasingly, the most sophisticated computer software deals
with _patterns_— for example, speech patterns and the
recognition of images. This chapter deals with the former: how
to recognize patterns of words and characters. Although you
can’t construct a human language translator with these
techniques alone, they are a start.

That’s what regular expressions are for. A _regular expression_
is a pattern you specify, using special characters to represent
combinations of specified characters, digits, and words. It
amounts to learning a new language, but it’s a relatively simple
one, and once you learn it, this technology lets you to do a great
deal in a small space—sometimes only a statement or two—that
would otherwise require many lines.

> > Regular expression syntax has a variety of flavors. The Python regular-expression
> package conforms to the Perl standard, which is an advanced and flexible version.

# ## 6.1 INTRODUCTION TO REGULAR EXPRESSIONS

A regular expression can be as simple as a series of characters