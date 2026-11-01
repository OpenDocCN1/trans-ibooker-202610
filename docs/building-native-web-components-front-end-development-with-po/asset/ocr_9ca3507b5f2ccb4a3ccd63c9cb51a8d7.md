# Polyfills

Polyfills are used to add missing features to web browsers, through libraries that simulate these capabilities. For web components, there is a reliable package that we can add to our projects to support more web browsers. You can access the project at https://github.com/webcomponents/polyfills/tree/master/packages/webcomponentsjs and add the polyfills quickly, using unpkg.com (Listing 7-5).

# *Listing 7-5.* Adding webcomponentsjs Polyfills

```
```html
<!DOCTYPE html>
<html>
<head>
<meta name="viewport" content="width=device-width, initial-scale=1">
<script async src="https://unpkg.com/browse/@webcomponents/webcomponentsjs@2.4.3/webcomponents-bundle.js"></script>
<script async type="module" src="http://unpkg.com/apress-simple-form-modal-component@1.0.1/src/simple-form-modal-component.js"></script>
<script async type="module" src="http://unpkg.com/apress-book-web-components-note-list@1.0.1/src/note-list-component.js"></script>
<script async type="module" src="http://unpkg.com/apress-note-list-item-component@1.0.1/src/note-list-item-component.js"></script>
<script async type="module" src="./app.js"></script>
<link rel="stylesheet" type="text/css" href="./style.css">
</head>
<body>
<h2>Notes App</h2>
<button class="fab" id="myBtn">+</button>
<simple-form-modal-component></simple-form-modal-component>
<note-list-component></note-list-component>
</body>
</html>
```
```

With this modification, we now have better support for web browsers lacking the Web Components APIs.