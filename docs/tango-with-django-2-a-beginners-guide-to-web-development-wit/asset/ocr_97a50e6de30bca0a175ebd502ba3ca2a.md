```
```javascript
$('p').hover(
    function() {
        $(this).css('color', 'red');
    },
    function() {
        $(this).css('color', 'black');
    }
);
```
```

Add this JavaScript to your `rango-jquery.js`. In the `about.html` template, you should then add a paragraph underneath your buttons – something like `<p>This is some text for the JQuery example.</p>` should suffice. Look at the code, and think what will happen... then try it out! Does it do what you expect to happen?

Here, we are selecting all the p HTML elements. We assign a `hover()` event to the elements. `hover()` requires two functions – one to be executed when you *hover over* something (the first function), and one to execute when you leave something (the second function). This means that when a user hovers over a p element, the text colour within the element will change to red. When the user’s cursor leaves the element, it changes to black. Have a look at the `hover()` event’s [documentation](https://api.jquery.com/hover/) for more information.

Note that we can use the `this` variable to reference *the element that is being hovered over*, and the `css()` function (part of JQuery) to set the color attribute for the said element to whatever we so choose (as we specify in the second parameter).

# 🔑 Remember – `ready()`!

Remember, it’s good practice to place your JQuery code within a `$(document).ready()` function call. Doing so will ensure that this code will only ever execute when the page has been completely loaded into memory, reducing the chances of your code breaking in very strange ways.

It takes time for content to download to a client’s computer; make sure your code is patient!