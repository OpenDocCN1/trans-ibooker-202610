# # Initializing our blog page

Scully provides a specific Angular CLI schematic for initializing an Angular application, such as a blog, by using Markdown (`.md`) files:

```
```
ng generate @scullyio/init:markdown
```
```

The previous command will start the configuration process of our blog by going through a list of questions (default values are shown inside parentheses):

+   1. Type `posts` as the name of the blog module:
+   
+       ```
+       ? What name do you want to use for the module? (blog)
+       ```
+   
+       Figure 2.5 – Blog module name
+   
+       This will create a new Angular module named `posts`.
+   2. Leave the slug choice empty, and press Enter to accept the default value:
+   
+       ```
+       ? What slug do you want for the markdown file? (id)
+       ```
+   
+       Figure 2.6 – Markdown slug
+   
+       The slug is a unique identifier for each post, and it is defined in the route configuration object of the module.
+   3. Enter `mdfiles` as the path that Scully will use to store our actual blog post files:
+   
+       ```
+       ? Where do you want to store your markdown files?
+       ```
+   
+       Figure 2.7 – Markdown files path
+   
+       This will create an `mdfiles` folder inside the root path of our Angular CLI project. By default, it will also create a blog post for our convenience. We will learn how to create our own in the Displaying blog data on the home page section.
+   4. Type `posts` as the name of the route for accessing our blog posts:
+   
+       ```
+       ? Under which route do you want your files to be requested?
+       ```
+   
+       Figure 2.8 – Blog route name
+   
+       The name of the route is the `path` property of the route configuration object that will be created.