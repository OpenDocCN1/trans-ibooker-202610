be read by the next regex matching or searching operation.

(?
<=ex
pr)
Positive look-behind. The overall expression matches if
immediately preceded by expr, which must be of fixed length.
The effect is to temporarily back up the appropriate number of
characters and reread them if possible. The characters reread
this way are not tagged.

For example, given the expression (?<=abc) def, the characters
def within abcdef are matched; however, only the characters
def are actually matched; the characters abc are not part of the
match itself. The pattern says, “Match def but only if preceded
by abc.”

(?
<!ex
pr)
Negative look-behind. The overall expression matches if not
immediately preceded by expr, which must be of fixed length.
The effect is to temporarily back up the appropriate number of
characters and reread them. The characters reread are not
tagged.

(?
P<na
me>e
xpr)
Named group. The overall expression matches if expr matches.
As a result, the group is tagged but also given a name so that it
can be referred to in other expressions by that name.

(?
P=na
me)
Test for a named group. This expression is a positive “match” if
the named group has previously appeared and been matched.

(#te
xt)
Comment. This text may appear within a regular expression, but
it will be ignored by the regular-expression evaluator itself.