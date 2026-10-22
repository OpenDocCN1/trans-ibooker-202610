

## Listing 10.3 templates/forms.html

```
<html>
<body>
<form action="/order" method="post">
    <select name="drink">
        <option value="">Choose drink --</option>
        <option value="coffee">Coffee</option>
        <option value="decaf">Decaf</option>
    </select>
    <input type="submit" value="Submit">
</form>
</body>
</html>
```

Simon started his explanation.

- 1. "Look at the number (1)," Simon said. "Here we define the *variable* we want to return from this menu. In this menu the variable is called `drink`.
- 2. "Now, on the line with number (2) and the line after it we define the *value* this menu option will return. It’s very similar to what we did when we chose items from the text menu, remember?"
- 3. "And in the line with number (3) I just added an option that tells the user what to do. This option will show first in the menu and work as a prompt. As you can see, its value is empty. If the user haven’t chosen a drink, we should tell them about it. You can’t prepare their order without this information, can you?"

"Can I try it?" Erik asked.

"Of course, go ahead and open this file again. Or just reload it in the browser."

Erik reloaded the file, chose "Decaf" from the menu, clicked "Submit" and got this:

![](img/f6295b544c84568800930d6ec95963ae_142_0.png)

# Your file couldn’t be accessed

It may have been moved, edited, or deleted.

ERR_FILE_NOT_FOUND

"What’s that?" he asked, visibly puzzled.

"Oh, I forgot to tell you," Simon said. "Look, I changed the form tag a little bit."

## Listing 10.4 templates/forms.html

```
<form action="/order" method="post">
```

"Each form should have an action," he started to explain. "Action is something that our application will do when the user submits the form. When the user made their choice—coffee or decaf—they should pass this information to some function. That function should know what to do with this information: store it in the file or database, print the order, and such."

"Like what we did in our previous program?" Erik asked.

"You keep talking about your 'previous' program, can you show it to me?" Emily demanded.

"I’m sorry, Emily," Simon said. "I should have explained it earlier. We worked with Erik on a program that collects orders in a coffee shop. Similar to Starbucks, where you can order a drink, add flavors and toppings, and all that. Erik wrote a program that shows menus and asks the customer what they want to order. When they choose their drinks, flavors, and toppings, the program prints the order. But the program now works only in a terminal, in text mode. Erik needs your help to convert it to a web application."

"I see now," Emily said. "This sounds like a cool project! I hope Erik will teach me Python too."

"Of course," Simon said. "Teaching somebody is the best way to learn."

"Back to our form," he continued. "That `action` attribute tells the browser: 'After the user submits the form, open this address and pass the information from the form there. In our case the address is called `/order`. Don’t worry, it only sounds scary, I’ll show you what to do with it," Simon added because he noticed the confusion on Emily and Erik’s faces.

"I still don’t understand," Erik asked. "Where is this address that you are talking about?"

"Look at your first application," Simon said. "See this `greet` function?" "This function was written by the Mu Editor for us—or, rather, its authors," Simon said. "You see now the familiar function definition that’s starting with `def`, but also, right above it, you can see something new: `@app.route('/hello/<name>')`. In Python it’s called *decorator*, but we are not going to learn about decorators today.

## Listing 10.5 first_app.py

```
@app.route('/hello/<name>')
def greet(name='Stranger'):
    return render_template("greeting.html", name=name)
```

What’s important for us today is that you can use it to tell your program which function to use for which address."

"Ah-ha, the address is that `/hello/Erik` that I entered in the browser, I see now!" Erik said. "Let’s show Emily how it works," and he opened the tab with the `first_app.py` script in the editor, clicked Run and then Browse. His browser opened a page with greetings from Mu.

"Emily, look, I can type the address here, right after these numbers: `127.0.0.1:5000` and look what it shows us!" Erik typed: `/hello/Emily`, pressed ENTER and the browser showed:

# Hello!

Hello Emily, how are you?

"Wow, I like it!" Emily said. "Can I try?" and she changed "Emily" to "Erik". Of course, the browser showed the page with "Hello Erik, how are you?"

"Interesting!" she said. "In our HTML class we could change pages, but we had to edit HTML. This is much easier!"

"Right," Simon said, "this is what we call *dynamic* pages—pages that change depending on what you enter. You can enter your information in the address, like `/hello/Emily` or you can use forms. Then the page will be *generated* using the information you entered. I'm sure, you've seen this kind of pages many times—for example when you enter a comment, or chat with someone on the web. You click 'Submit' or just press ENTER and the page is updated, right? Now you will learn how to make such pages yourself."

"Let me show it on a diagram," he said and started drawing.

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

And Simon wrote the function, looking at the example he kept opened in the browser. He added numbers at the end of the lines to help him in his explanation.

"At the line with (1) you see two words: 'GET' and 'POST'. These are the methods that we use with web servers. We use GET when we want to get something from a web server, like a web page. We use POST to send some information to the web server. Like in this case—we want to send or POST the drink chosen by the customer. Put it another way: when you load a page in the browser—you use GET; when you click Submit in your form—you use POST. You will understand it better when we start using it, don’t worry.

"And here, at the line with number (2), we start using one of the words. Look what it says: if the method is POST--which means somebody filled the form and clicked Submit—we read the information they entered in the form and print it.

"Now look at the number (3). Remember in the form we used <select name="drink">? This is the name we use here, in the square brackets. Later we’ll add other menus—for flavors and toppings. In the form they will have names like 'flavor' and 'topping'. Here in the code we’ll use them as request.form['flavor'] and request.form['topping']."

"Under number (4) we just print whatever we received from the form. You will see it in the editor.

"In the line with number (5) we tell our web server to print this page with the menu. It’s like a menu loop that we used in our program before—you get the information from the customer, print it out and return to the menu to get another order. And you repeat this loop until you are done entering orders."

## Listing 10.6 first_app.py

```
@app.route('/order', methods=('GET', 'POST'))
①
def order():
    if request.method == 'POST':
        ②
        drink = request.form['drink']
        ③
        print("Drink: ", drink)
        ④
    return render_template("forms.html")
    ⑤
```

- ① Methods that we are going to use with this form
- ② Method POST means we are submitting information
- ③ We get the customer choice from the form’s field called drink
- ④ Print the choice we’ve received
- ⑤ Display the template forms.html

"Let me add one more thing," Simon said and added request to the first line with import. "This

