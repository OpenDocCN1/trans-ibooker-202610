page? That’s the advantage of using the style sheets `normalize.css` and `skeleton.css`. We just added a couple of little changes with our `coffeeshop.css.`

"It’s great that customers in our coffee shop can order drinks from their phones," Emily said. "But for that, they should be in the shop and connected to our network, right? Simon, can we place our application in the real cloud, so we can use it from anywhere? I would love to show it to my friends at school."

"Sure, it’s possible. Of course, it’s a bit more complicated than just adding one string like we just did. Usually, cloud services cost some money, but for simple projects like ours it’s possible to find a free service. One of them is called PythonAnywhere. I know that they can host your Python application but I haven’t tried it myself. Give me some time and I’ll test it and give you the instructions."

NOTE You will find the instructions on the book companion website at https://github.com/pavelanni/lets-talk-python-book/tree/main/ch15/;

"But before we open the application to the whole world, let’s talk about possible improvements. Erik, do you remember what we discussed when working with the database?"

"You said it would be good to include the time of the order in the database table," Erik said.

"Yes, right! Let’s start a ToDo list to plan our future improvements," and he started writing in a text editor.

```
* Timestamps in orders
```

"We can remember each customer," Emily said, "and show them what they ordered last time."

"Good idea," Simon said. "But in this case we should create a unique name for each customer. We don’t want to mix two different customers whose names are Alex."

"What about our menus?" Erik asked. "When we run it on my laptop, we can edit the text files. But when we run it on the cloud, we can’t edit the files, can we?"

"Yes, you are right, Erik," Simon said. "If we go to the cloud, everything should be done via a web browser. We need another page to edit menus. And, don’t forget that this page should be used only by shop managers."

"I see what you mean!" Emily said. "We need passwords!"

"Right," Simon confirmed. "We need different *accounts* in our application. We’ll have customer accounts, then we’ll have barista accounts where they will receive orders. And we’ll have shop manager accounts where they’ll list orders, edit menus, and do other manager’s things."