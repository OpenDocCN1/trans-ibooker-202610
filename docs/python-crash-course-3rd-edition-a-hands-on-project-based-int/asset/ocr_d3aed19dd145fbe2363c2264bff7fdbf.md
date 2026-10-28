# base.xhtml

```
1 <!doctype html>
2 <html lang="en">
3 <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
4     <title>Learning Log</title>

5     {% load django_bootstrap5 %}
    {% bootstrap_css %}
    {% bootstrap_javascript %}

</head>
```

We first declare this file as an HTML document ① written in English ②. An HTML file is divided into two main parts: the head and the body. The head of the file begins with an opening <head> tag ③. The head of an HTML file doesn’t hold any of the page’s content; it just tells the browser what it needs to know to display the page correctly. We include a <title> element for the page, which will display in the browser’s title bar whenever Learning Log is open ④.

Before closing the head section, we load the collection of template tags available in django-bootstrap5 ⑤. The template tag {% bootstrap_css %} is a custom tag from django-bootstrap5; it loads all of the CSS files required to implement Bootstrap styles. The tag that follows enables all the interactive behavior you might use on a page, such as collapsible navigation bars. The closing </head> tag appears on the last line.

All Bootstrap styling options are now available in any template that inherits from base.xhtml. If you want to use custom template tags from django-bootstrap5, each template will need to include the {% load django_bootstrap5 %} tag.