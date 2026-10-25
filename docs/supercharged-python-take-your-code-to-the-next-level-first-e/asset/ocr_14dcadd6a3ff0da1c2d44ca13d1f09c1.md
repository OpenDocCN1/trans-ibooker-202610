elements in lowest-to-highest order by default—or by highest-to-lowest if `reverse` is specified and set to **True**. If the list consists of strings, the strings are placed in alphabetical (code point) order.

The following example program prompts the user for a series of strings, until the user enters an empty string by pressing Enter without any other input. The program then prints the strings in alphabetical order.

# Click here to view code image

```
```
def main():
    my_list = []   # Start with empty list
    while True:
        s = input('Enter next name: ')
        if len(s) == 0:
            break
        my_list.append(s)
    my_list.sort()   # Place all elems in order.
    print('Here is the sorted list:')
    for a_word in my_list:
        print(a_word, end=' ')
main()
```
```

Here’s a sample session of this program, showing user input in bold.

```
```
Enter next name: **John**
Enter next name: **Paul**
Enter next name: **George**
Enter next name: **Ringo**
Enter next name: **Brian**
Enter next name:
Here is the sorted list:
```
```