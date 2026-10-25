```
try:
    f = open('silly.txt', 'r')
    text = f.read()
    print(text)
except IOError as e:
    print('Problem opening file:', e)
except Exception as e:
    print('That was a Bozo no-no.')
    print('Error text:', e)

while condition:
    statements
[else:
    statements] # Executed if first block of statements
                # finished successfully, without break

with open('stuff.txt', 'r') as f:
    print(f.read())  # Print contents of a file.
```