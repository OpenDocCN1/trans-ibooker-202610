```
pat = r'. *?[.?!]'  # Notice use of NON-GREEDY
    # because of the first "?".
lst = re.findall(pat, s, flags=re.DOTALL)
print('There are', len(lst), 'sentences.')

The U.S.A. has 310.5 million people.

s = '''See the U.S.A. today. It's right here, not
    a world away. Average temp. is 66.5.'''

import re
pat = r'[A-Z].*?[.!?](?= [A-Z]|$)'
m = re.findall(pat, s, flags=re.DOTALL | re.MULTILINE)

See the U.S.A. today. It's right here, not

pat = r'[A-Z].*?[.!?] [A-Z]|$'
m = re.findall(pat, s, flags=re.DOTALL)
for i in m:
    print('->', i)

pat1 = r'( \w|[!@#$%^&*+-]){8,12}$'
pat2 = r'(?=.*[a-zA-Z])'  # Must include a letter.
pat3 = r'(?=.*\d)'  # Must include a digit.
pat4 = r'(?=.*[!@#$%^&*+-])'  # Must include punc. char.

pat = pat2 + pat3 + pat4 + pat1
```