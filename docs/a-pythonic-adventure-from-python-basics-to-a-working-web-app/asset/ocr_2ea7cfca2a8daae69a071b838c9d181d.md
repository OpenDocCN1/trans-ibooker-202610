address or URL

browser

http://127.0.0.1:5000/order

- Choose drink -

Submit

Python

```
@app.route('/order')
def order():
    function code
```

HTML

```
<form action="/order" method="post">
  <select name="drink">
    <option value="">- Choose drink -</option>
```

> "This combination of letters and numbers at the top of your browser is called 'address' or 'URL'. I marked it blue. Usually you see the site’s name here, like google.com. In our case it uses numbers called the site’s IP address. We use your own computer and for every computer in the world the address 127.0.0.1 means 'this computer.' But don’t worry about that now."

Simon pointed to the first red circle around the word 'order' in the address and said: "This is what we should care about. Look, it’s part of the address. When we open this address we see the form with the drinks menu. When you click Submit," and he followed the arrow down his diagram, "the form knows that it should find the function responsible for the address /order. You see, it’s here, in the form action field.

And then," he followed the arrow up to the 'Python' block, "the form finds the Python function that can work with it, because we used this decorator, @app.route('/order'). You see, these three things are connected; you just have to use the same name in the address, in the form, and in the Python program."

"I see that the function is also called 'order'--is it the fourth place where we use it?" Erik asked.

"You have a very sharp eye!" Simon said and smiled. "No, in this case, the function can have a different name. I could call it 'new_order' or 'get_order'. But now we have to write the actual function. I’ll help you here. It will look a bit scary, but don’t worry. I’m learning this Flask system myself and usually I follow the online tutorial and take examples from there. Don’t think that I remember all these things myself."