![](bbox=[0.13, 0.05, 0.39, 0.5])

"Let's start with get_name. What do we do here?" Simon asked.

"We just ask 'what is your name?'," Erik answered.

"Right, and then?"

"And then we save it in a variable, like name."

"Almost right," Simon said. "Remember, we decided that order will be a dictionary. And we will save everything related to that order in this dictionary. For example, to save the customer's name instead of name = 'Erik' we should write order['name'] = 'Erik'. Only instead of 'Erik' we will use the function input() like you did in your first program."

"Let me try," Erik said and wrote this function:

```
def get_order():
    order['name'] = input("What's your name: ")
    return {}
```

"Now try it," Simon said.

Erik clicked Run, the program asked his name and he entered 'Erik'. But then he's got several