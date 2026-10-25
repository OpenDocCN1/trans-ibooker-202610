```
frank_str = 'doo be doo be doo...'
n = -1
while True:
    n = frank_str.find('doo', n + 1)
    if n == -1:
        break
    print(n, end=' ')

frank_str = 'doo be doo be doo...'
print(frank_str.rfind('doo'))  # Prints 14.

title = '25 Hues of Grey'
new_title = title.replace('Grey', 'Gray')

title = 'Greyer Into Grey'
new_title = title.replace('Grey', 'Gray')

input_str.split(delim_string=None)

stooge_list = 'Moe Larry Curly Shemp'.split()
['Moe', 'Larry', 'Curly', 'Shemp']

stooge_list = 'Moe   Larry Curly   Shemp'.split()

stooge_list = 'Moe   Larry Curly   Shemp'.split(' ')
['Moe', '', '', '', 'Larry', 'Curly', '', '', 'Shemp']

stooge_list = 'Moe, Larry, Curly, Shemp'.split(', ')
```