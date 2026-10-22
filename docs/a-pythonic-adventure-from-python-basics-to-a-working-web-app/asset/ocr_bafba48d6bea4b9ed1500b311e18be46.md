# Thanks for your order!

Your drink: **coffee**

"Yeah, it works!" she exclaimed.

"It’s like calling the `print_order()` function in our previous program," Erik said.

"Yes, exactly!" Simon said.

"But how do I get back to the order page?" Erik asked.

"You see, the address in the browser is still pointing to `/order`. That means if you click the address line with mouse and press ENTER you’ll reload the order page. Just don’t click the reload button or it will create another order."

Emily did what Simon said and saw the order page again.

"But there is a better way," Simon said. "You were looking for a button on the print page, like Back to the order page, weren’t you?"

"Yes, that would be easier," Emily agreed.

"We can use another form for that," Simon said. "It will be very simple," and he added several lines to the `templates/print.html` file.

"Look, we created another form that has only the Submit button. We just renamed it to 'New order'. Look, it points to `/order` in its `action` field. That means when we click the 'New order' button it will send us to the `/order` page. And it will show the drink menu again. Try it!"