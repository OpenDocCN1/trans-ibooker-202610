# Hello Emily!

# Here is your order:

Drink: **chocolate**

Flavor: **vanilla**

Topping: **caramel**

"Wow, it works!" she was excited to see a real web application they just wrote together.

"This is great," Simon said. "Now we have to add the functions we wrote for the text version."

"What functions?" Emily was puzzled. She thouhgt it’s done already. "Erik, can you explain?"

"What we have written here," he started, "is a user interface of our application. We did it before, but in a text mode. This web application looks so much better. But in addition to printing the order we also want to save it in a file. We may want to count how many orders we served with caramel, for example."

"I see, to know when to buy more caramel for the shop," Emily said.

"Exactly! You think like a shop manager, Emily!" Simon said.

He continued: "Just before you joined us we created a function to save the orders in a file. Our program saved the orders when you told it you want to finish. Next time you started the program it loaded all the previous orders into a list to add new orders to it."

Simon stopped for a while, thinking. "But with this new web application we have to do it slightly differently."

"Why?" Erik asked.

"Because our application is running all the time and we are not going to stop it. This kind of application is called a *web service*. It doesn’t stop, it runs all the time. Even more, if something happens to it and it crashes for some reason, another program restarts it to make sure it is always ready to respond to requests-- in our case, to accept orders."

"For us," he continued, "it means that we have to save each order right after we receive it. Let me quickly find the right way of doing it."