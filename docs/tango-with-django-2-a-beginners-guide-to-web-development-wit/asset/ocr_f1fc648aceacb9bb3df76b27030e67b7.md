# Hint

This is just a straightforward HTTP GET request, like any request *your browser issues when you type in a URL or click a link*. What happens when you point your browser to `http://127.0.0.1:8000/rango/suggest/?suggestion=pro`?

# Adding AJAX to Request Suggestions

If you tested your server-side functionality and are happy with it, now it’s time to glue the left-hand side component of Rango’s pages to the server. We’ll do this by adding some more code to the `static rango-ajax.js` file. Note that you should add this *within* the `$(document).ready(function() { ... }` block, to ensure that all parts of the page are loaded before the code can be executed.

```
```javascript
$('#search-input').keyup(function() {
    var query;
    query = $(this).val();

    $.get('/rango/suggest/',
          {'suggestion': query},
          function(data) {
              $('#categories-listing').html(data);
          }
    );
});
```
```

This code binds a keypress event (when a user presses a key on their keyboard-/smartphone screen) when the `search-input` element is focused. In other words, when the user types a letter into the search box, the code is fired. The value of the input box (`$(this).val()`) is then extracted, and passed to an AJAX GET request. We wrap the string provided up by the user in a dictionary-like object, again to help construct the correct URL and querystring.

Once the server responds, the anonymous function is called, with data containing the response – or list of categories that match the user’s query. This list is then simply placed inside the `categories-listing <div>` element we updated earlier – replacing any existing content within the `<div>`. Have a look at the screenshots below to see the new functionality in action.