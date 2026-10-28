2. Run the following build command of the Angular CLI to build the application:

```
ng build ui-elements --single-bundle
```

The previous command will build the ui-elements application and produce a single bundle for all application code.

3. Copy the dist\ui-elements folder to another location of your choice in your hard disk and open the index.html file, using your integrated development environment (IDE).

4. Remove the <base> tag from the head element and add the Bulma CSS minified file using a content delivery network (CDN), as follows:

```
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.2/css/bulma.min.css">
```

5. Replace the app-root selector with the following HTML snippet in the body element:

```
<div class="container is-fluid">
<h1 class="title">My Angular Element</h1>
<copy-button></copy-button>
</div>
```

In the preceding snippet, we added a div element styled with Bulma CSS classes, and the selector of the Angular element that we defined in AppModule.

6. Insert the following JavaScript code after the div element:

```
<script>
const el = document.getElementsByTagName(
'copy-button')[0];
el.setAttribute('data', 'Some data');
el.addEventListener('copied', () => alert('Copied to
clipboard'));
</script>
```

In the preceding script, we communicate with the component that is hidden behind the Angular element using vanilla JavaScript. First, we query the global document object to get a reference to the Angular element. Then, we set the data input property using the setAttribute method of the element. Finally, we listen for the copied output event by attaching an event listener using the addEventListener method.