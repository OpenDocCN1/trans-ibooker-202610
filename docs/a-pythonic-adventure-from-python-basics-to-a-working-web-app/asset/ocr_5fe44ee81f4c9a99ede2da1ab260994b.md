the program. And in that case we collect the data from the form—the drink choice—and use *another* template. I called it print.html because we want to print the order."

The order() function now looked like this:

```
@app.route('/order', methods=('GET', 'POST'))
def order():
    if request.method == 'POST':
        drink = request.form['drink']
        print("Drink: ", drink)
        return render_template("print.html", drink=drink)

    return render_template("forms.html")
```

"But we don’t have a file called print.html," Emily said.

"Right, I am going to create it right now," and Simon created another file in the editor and saved it as print.html under templates.

Emily looked at his code and said: "Oh, this I can understand! You print the header 'Thanks for your order' and then you open a new paragraph and print 'Your drink' and then in **bold** you print the drink itself. And this drink in double curly braces works the same way it worked with my name when it printed 'Hello Emily', right?"

"Exactly right!" exclaimed Simon. "You are absolutely correct, Emily!"

# Listing 10.7 templates/print.html

```
{% extends "base.html" %}
{% block content %}
<h1>Thanks for your order!</h1>
<p>Your drink: <strong>{{drink}}</strong></p>
{% endblock %}
```

YOUR TURN   Edit your web form to print the drink choice on the page
Copy the template above into your print.html file. Feel free to change the title and the text. Change the first_app.py by adding the line with return and save it too. Try to run your program.

"Can I try it?" Emily asked.

"Sure, go ahead and click Run," Simon said.

Emily clicked Run, chose "Coffee" from the menu and saw this page: