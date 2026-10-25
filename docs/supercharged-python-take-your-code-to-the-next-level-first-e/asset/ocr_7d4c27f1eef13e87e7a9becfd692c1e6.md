```
s = 'elephant'
if s[0] in 'aeiou':
    print('First char. is a vowel.')

s = 'Helephant'
if s[0] not in 'aeiou':
    print('First char. is a consonant.')

if s[0] in 'aeiouAEIOU':
    print('First char. is a vowel.')

s = 'elephant'
if s[0].upper() in 'AEIOU':
    print('First char. is a vowel.')

print('' in 'cat')  # Prints True
print([] in [1, 2, 3])  # Prints False

s = 'Cat'
for ch in s:
    print(ch, ',', 'type:', type(ch))
```