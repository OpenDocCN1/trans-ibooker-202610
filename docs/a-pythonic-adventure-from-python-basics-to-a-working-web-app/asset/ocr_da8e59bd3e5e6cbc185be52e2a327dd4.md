loop will never end. In our example, we subtract 1 from n each time we go through the loop. So, eventually, the variable n will become equal to zero and the loop will stop.

"The important thing is that in this case, before we start the loop, we already know what is in the variable n and we know that it’s greater than zero.

"What if we don’t know what’s in the variable or the variable doesn’t even exist? Like in our case: we can check the user’s answer only after we ask to choose one of the items from the menu. So we have to do this," and Simon drew another diagram.

![](bbox=[0.15, 0.27, 0.85, 0.6])

# EXAMPLE

```
while True:
    answer = input()
    if answer == '1':
        break
```

Simon continued his explanation, "Here we start the loop without checking any condition. We do something first and only after that, we check the condition. Sometimes this loop is called 'do-until,' which means 'do something until the condition is true.' When the condition is true, exit the loop and continue with the rest of the program.

In the example here I used your situation with input (). You ask for input, then you check that input. This is your condition: if the input is valid, you should exit the loop. In Python we use the