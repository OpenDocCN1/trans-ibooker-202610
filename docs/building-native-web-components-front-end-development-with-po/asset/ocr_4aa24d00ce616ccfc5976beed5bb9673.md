```
<script async type="module" src="http://unpkg.com/apress-note-list-item-component@1.0.1/src/note-list-item-component.js"></script>

<script async type="module" src="./app.js"></script>

<link rel="stylesheet" type="text/css" href="./style.css">
</head>
...
```

With this, each time we run a new build, we can test whether the compiled file is still working as expected.

Now you can run the command $npm run start and go to localhost:5000, to see if everything is working as expected (Figure 7-11).

![](bbox=[0.1, 0.38, 0.92, 0.8])

Figure 7-11. Running simple-form-modal from a compiled file

It’s working as expected—and now we can use this in older web browsers, such as IE 11—and is using ES5, a JS specification that IE 11 should understand.