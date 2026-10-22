```
. . .
    while True:
        choice = input(prompt)
        if choice not in ['1', '2', '3']:
            answer = choices[int(choice) - 1]
            break
        else:
            print("Enter number 1, 2, or 3!")
            answer = ''
. . .
```

"This needs some explanation," he said. "Look, the user enters a string that can be '1' or can be '42'. We test whether this string is in the list of allowed answers, which is 1, 2, 3. If it’s not in the list, then we print the error message and continue with the loop. If it is in the list of allowed answers, then we convert it and pick that item from the menu list."

"Yes, I understand," Erik said. "But what about the second menu where I got the error? It didn’t let me choose number 4 because it wasn’t in my if. With your list I will have the same problem. I have five flavors, so I need another list of answers here, right?"

"Excellent question!" Simon said. "I was just about to ask it myself. Yes, you are right. Each menu list should have its own list of allowed answers. Not a big problem; we can build one when we know what’s in our menu list. Let me write it first and then I’ll explain it step-by-step." And Simon added the following lines just before the if block he added previously:

```
. . .
    while True:
        choice = input(prompt)
        allowed_answers = [] ①
        for a in range(1, len(choices)+1): ②
            allowed_answers.append(str(a)) ③
        if choice not in allowed_answers:
            answer = choices[int(choice) - 1]
            break
        else:
            print("Enter number 1, 2, or 3!")
            answer = ''
. . .
```

+   ① Empty list for allowed answers

+   ② Calculate the length of the menu

+   ③ Append the allowed answer

"First (look at the label 1) we create an empty list for allowed answers. Then (label 2) we measure the length of the menu list using the function len(). For your drinks, the result will be three, and for the flavors it will be five. Then we use the function range() to create a sequence of numbers from one to the length of the menu. For drinks the sequence will be 1, 2, 3. For flavors it will be 1, 2, 3, 4, 5. You get the idea. Just notice that in the range() function we shouldn’t use the last element of the sequence, but the one after the last which is not included in the sequence. That’s why we have to add one to the length of the menu like this:"