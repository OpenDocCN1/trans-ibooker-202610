The break statement ends a loop when a certain condition occurs:

```
while True:
    name = input("What's your name? ")
    if name == 'quit':
        break
    print(f"Hello, {name}!")
```

```
What's your name? Cedar
Hello, Cedar!
What's your name? quit
```

The continue keyword breaks the loop and returns to the beginning of the loop:

```
while True:
    age = input("How old are you? ")
    age = int(age)
    if age < 0:
        print("That makes no sense!")
        continue
    print(f"{age} is a great age!")
```

```
How old are you? 17
17 is a great age!
How old are you? -5
That makes no sense!
How old are you? 5
5 is a great age!
```