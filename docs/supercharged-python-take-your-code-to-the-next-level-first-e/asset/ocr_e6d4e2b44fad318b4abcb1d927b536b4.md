Public, John Q.

It’s then an easy matter to order them alphabetically by last name if desired. But what if someone enters a name without a middle initial?

Jane Austen
Mary Shelley

Ideally, we’d like to handle those as well. We’d like a pattern that smoothly handles both kinds of cases—middle initial present or not present.

Austen, Jane
Shelley, Mary

So let’s start with the simple case: first and last name only. It’s particularly convenient to tag two groups and give them the names `first` and `last`, as in the following pattern.

Click here to view code image

pat = r'(?P<first>\w+) (?P<last>\w+)'

We can successfully apply this pattern in a program in which a person enters their full name, to be broken down and analyzed.

import re
s = 'Jane Austen'
m = re.match(pat, s)