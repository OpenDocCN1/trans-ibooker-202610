```
print(a, ' plus ', b, ' equals ', c, '.', sep='')
print('%d plus %d equals %d.' % (a, b, c))
>>> 'Here is a number: %d.' % 100
'Here is a number: 100.'
format_str % value    # Single value
format_str % (values) # One or more values
print('n is %d' % n)
print('n is %d and m is %d' % (n, m))
tup = 10, 20, 30
print('Answers are %d, %d, and %d.' % tup)
h1 = int('e9', 16)
h2 = int('10', 16)
print('The result is %x.' % (h1 + h2))
print('The result is %x.' % h1 + h2)    # ERROR!
print('The result is 0x%x.' % (h1 + h2))
s = 'We is %s, %s, & %s.' % ('Moe', 'Curly', 'Larry')
print(s)
```