```
def main():
    age = int(input('Enter your age: '))
    name = input('Enter your name: ')
    if age < 30:
        print('Hello', name)
    print('I see you are less than 30.')
    print('You are so young.')
```

```
>>> def main():
    side1 = float(input('Enter length of a side: '))
    side2 = float(input('Enter another length: '))
    hyp = (side1 * side1 + side2 * side2) ** 0.5
    print('Length of hypotenuse is: ', hyp)
```

```
>>> main()
Enter length of a side: 3
Enter another length: 4
Length of hypotenuse is: 5.0
>>> main()
Enter length of a side: 30
Enter another length: 40
Length of hypotenuse is: 50.0
```

```
if a > b:
    print('a is greater than b')
    c = 10
```