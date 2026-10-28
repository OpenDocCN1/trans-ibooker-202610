# Play Around

Play around with the code you have just written. Have a look at the [jQuery documentation](https://api.jquery.com/category/events/mouse-events/) to see what else you can do. And one little thing: what do you think will happen if you change the two instances of `$(this)` above to `$('p')`? Think about it, then once you have worked out a solution, experiment and see if you were correct!

# 16.3 Further DOM Manipulation Examples

In the above example, we used the `hover()` function to assign an event handler to the hover in and hover out events. For each of those events, we then used the `css()` function to change the text colour of the element being hovered over. The `css()` function is one example of *DOM manipulation* – or the changing of the page loaded dynamically. However, `css()` is not the only way you can manipulate a page. jQuery provides other ways to manipulate the page!

For example, we can remove a class from elements with the `removeClass()` function, and add classes to elements with the `addClass()` function.

```
```javascript
$("#about-btn").removeClass('btn-primary').addClass('btn-success');
```
```

This is an example of *chaining* calls. This code selects the `about-btn` element (by its unique ID), and removes the `btn-primary` class from the element. It then adds a new class, `btn-success`. This has the effect of making the button turn from blue to green. Why? The Bootstrap toolkit implements these classes – `btn-success` just so happens to set the background colour of the element to green.

It is also possible to access the *inner HTML* of a given element – or, in other words, the markup nested within a given element. For example, let’s put a new `<div>` element in Rango’s `about.html` template, underneath our sample `<p>` element.