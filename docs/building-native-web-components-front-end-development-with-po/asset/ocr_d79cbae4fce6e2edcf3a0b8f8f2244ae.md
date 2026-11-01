Here, we are saying take the file in ./src/index.js and resolve all the dependencies, put them in ./dist/index.js, and handle that as a library in umd format. Universal Module Definition, or UMD, is a way to create modules before JS modules were added in web browsers.

Now we will add some npm scripts in our project (Listing 7-7).

# Listing 7-7. Adding npm Scripts in package.json

```
"scripts": {
  "clean": "rimraf dist",
  "build": "npm run clean && webpack --mode production",
  "cpdir": "copyfiles -V \"./dist/*.js\" \"./example/\"",
  "start": "npm run build && npm run cpdir && serve example"
}
```

clean removes any generated code from the previous build. cpdir copies the ./dist directory inside the ./example folder. build uses webpack to create the transpiled file and start to run a local server to see our compiled component running in a sample.

Now I am going to create a sample folder and copy in it our complete NoteApp project, as you can find in the branch chap-7-5. To test our build, we must modify the index.html, as in Listing 7-8.

# Listing 7-8. Loading Our simple-form-modal-component from ./dist/index.js

```
<!DOCTYPE html>
<html>

<head>

<meta name="viewport" content="width=device-width, initial-scale=1">

<script async src="https://unpkg.com/browse/@webcomponents/webcomponentsjs@2.4.3/webcomponents-bundle.js"></script>

<script async src="./dist/index.js"></script>

<script async type="module" src="http://unpkg.com/apress-book-web-components-note-list@1.0.1/src/note-list-component.js"></script>
```