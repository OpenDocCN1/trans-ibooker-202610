```
1 <!DOCTYPE html>
2 {% load staticfiles %}
3 
4 <html>
5   <head lang="en">
6     <meta charset="UTF-8" />
7     <title>Rango</title>
8   </head>
9   <body>
10     <!-- Page specific content goes here -->
11   </body>
12 </html>
```

For the time being, let’s make this simple HTML page our app’s base template. Save this markup in Rango’s `base.html` template.

# DOCTYPE goes First!

Remember that the `<!DOCTYPE html>` declaration **always needs to be placed on the first line** of your template. Not having a document type declaration³ on the first line may mean that the resultant page generated from your template will not comply with W3C HTML guidelines⁴.

# Template Blocks

Now that we’ve created our base template, we can add template tags to denote what parts of the template can be overridden by templates that inherit from it. To do this, we will be using the `block` tag. For example, we can add a `body_block` to the base template in `base.html` as follows: