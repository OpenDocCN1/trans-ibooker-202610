have a folder named *learning_logs* inside a folder named *templates* inside a folder named *learning_logs*), but it sets up a structure that Django can interpret unambiguously, even in the context of a large project containing many individual apps. Inside the inner *learning_logs* folder, make a new file called *index.xhtml*. The path to the file will be *ll_project/learning_logs/templates/learning_logs/index.xhtml*. Enter the following code into that file:

# *index.xhtml*

```
<p>Learning Log</p>

<p>Learning Log helps you keep track of your learning, for any topic you're
interested in.</p>
```

This is a very simple file. If you’re not familiar with HTML, the <p></p> tags signify paragraphs. The <p> tag opens a paragraph, and the </p> tag closes a paragraph. We have two paragraphs: the first acts as a title, and the second describes what users can do with Learning Log.

Now when you request the project’s base URL, *http://localhost:8000/*, you should see the page we just built instead of the default Django page. Django will take the requested URL, and that URL will match the pattern ''; then Django will call the function `views.index()`, which will render the page using the template contained in *index.xhtml*. Figure 18-3 shows the resulting page.