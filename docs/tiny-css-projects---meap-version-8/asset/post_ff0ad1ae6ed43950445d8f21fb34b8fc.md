
selectors, we write them as a comma delimited list before the opening curly bracket. To select all h1 and h2 elements for example, we would write h1,h2 { ... }.

The declaration is made up of the property, in this case color, followed by the property value (red). The declaration defines how the element selected will be styled.

Both properties and values must be written in American English. Spelling variations such as "colour" or "capitalise" are not supported and will not be recognized by the browser.

When a browser comes across invalid CSS, it will ignore it. If a rule has an invalid declaration inside of it, valid declarations will still be applied, only those that are invalid will be ignored.

Internal CSS works well for one-off web pages where the styles are specific to that page. It groups CSS nicely, allowing us to write rules that are applied across elements, preventing us from having to copy and paste the same styles in multiple places. It also has the same performance benefits as inline styles in that the browser immediately has access to the CSS rather than having to wait for it to be fetched from a different location.

The downside of having our CSS within our HTML document is that the CSS will only work for that document. So if our website has multiple pages, which is often the case, we would need to copy that CSS into each HTML document. This will quickly become unmaintainable especially for large applications such as blogs or ecommerce websites because any style change would have to be applied to all pages.

Let’s undo our changes to our project one last time, and look at a third technique.

## 1.3.3 External CSS

Like internal CSS, the external CSS approach keeps our CSS grouped together however it places the CSS in a separate .css file. By separating our HTML and CSS, we can effectively separate our concerns: content and style.

We link the stylesheet to the HTML by using the <link> HTML tag. The link element needs two attributes for stylesheets: the rel attribute, which describes the relationship between the HTML document and the thing being linked to; and the href attribute which stands for hypertext reference and indicates where to find the document that we want to include. Listing 1.2 show how we link our stylesheet to our HTML for our project.

```
Listing 1.3
<!DOCTYPE html>
<html>
<head>
<link rel="stylesheet" href="styles.css">
</head>
<body>
<h1>Inline CSS</h1>
</body>
```

Most of the time this is the approach you will see across the web and is the approach we will use throughout this book. The benefit of external stylesheets is that our CSS is in one single document which can be modified once and whose changes will apply across all of our HTML pages. The downside to this approach is that it does mean it will take an extra request from the browser to retrieve that document, losing the performance benefit provided by putting the CSS directly inside of the HTML.

## 1.4 The cascade of cascading style sheets

One of the fundamental points to understand about CSS is the cascade. When CSS was formed, it was around the concept of cascading, which allows styles to overwrite or inherit from each other. This then paved the way for multiple stylesheets which compete over the presentation of the web page.

Because of this, while inspecting an element using the browser's developer tools, we can sometimes see multiple CSS values fighting over each other to be the one rendered by the browser. How the browser decides which CSS property values to apply to an element is through specificity.

Specificity is when the browser (or the user-agent) decides which declarations are relevant to the HTML and applies the styling to that element. One aspect in which specificity is calculated is the order in which style sheets are applied. When multiple stylesheets are applied the styles in the later will override styles provided by the previous stylesheet. In other words, the last one declared wins.

In CSS there are three different stylesheet origins.

### 1.4.1 User-agent style sheets

The first origin is the browser's default styles. When we first opened the project, before we added any styles to it, our elements did not all look the same. Our headers are bigger and bolder than our text for example. This is defined by the user-agent (UA) style sheet. These have the lowest priority and we find that different browsers present HTML properties slightly differently.

Most of the time UA stylesheets set the font size, border styles, and some basic layout for form elements, like the text input or progress bar. This can be useful if the user stylesheet cannot be found or there is an error loading the file. The UA stylesheet provides some fallback styling, which allows the user to still use the website.

### 1.4.2 Author stylesheets

The stylesheets we as developers write are known as the author stylesheets and they typically have the second highest priority in terms of the styles the browser displays. When we create a web page the CSS stylesheets we write and apply to our web pages are Author stylesheets.

### 1.4.3 User stylesheets

The user who is accessing our web page can use their own stylesheet to override both author and UA styles. This can improve their experience, especially for disabled users.

Instances of where a user may use their own style sheets include to set a minimum font size, choose a custom font, improve the contrast or increase the spacing between elements. Any user can apply a user stylesheet to a webpage.

How these are applied to the webpage will depend on the browser, usually through the browser settings or by using a plugin.

Once applied the, the user stylesheet is only applied for the user that added it, on the browser they applied it to. Whether the change is carried over from one device to another, will again depend on the browser itself and its ability to sync plugin and setting cross devices.

### 1.4.4 CSS reset

Default styles provided by the browser are not consistent. Each browser has their own stylesheets. Default styles are different in Chrome than they are in Safari for example. This can create some challenges if we want our applications to look the same across all browsers. Luckily for us there are two options available, CSS Resets or Normalize. Although either can be used to solve cross browser styling issues, they work in radically different ways.

Reset says we don't want any of the browser default styling at all and resets them all. Without any author styles applied, all elements regardless of what they are, look like plain text as we can see in Figure 1.7.

![](img/b56145aa9b751926a7aedbd7172bc818_18_0.png)

Figure 1.7 CSS Reset applied

To apply CSS Reset to our project first we create a reset stylesheet to add to our project. In our project folder we create a file called reset.css. We then copy the reset CSS into the file.

Many reset options exist but a commonly used option can be found at https://meyerweb.com/eric/tools/css/reset/.

Finally we need to link our stylesheet to our HTML. Because order matters, we want to make sure we include the reset CSS **before** our author styles in our `head`. Our HTML will therefore look as seen in Listing 1.3.

```
Listing 1.4
<head>
...
<link rel="stylesheet" href="reset.css"> #A
<link rel="stylesheet" href="styles.css"> #B
</head>
```

#A Reset stylesheet
#B Author stylesheet

The benefits of CSS Reset is that we now have a blank slate to start from. As seen in Figure 1.7 all or our elements now look like plain text so the downside is that we now need to define basic styles for all elements including adding bullets to lists, differentiating header levels, etc. Furthermore, each version of CSS Reset will be slightly different based on the version and the developer who authored it.

Our other option is Normalize. Instead of resetting the styles, it specifically targets elements that have differences across browsers and applies rules to standardize them.

### 1.4.5 Normalize

Like CSS Reset, Normalize will style things slightly differently depending on the version and author. A commonly used option for Normalize can be found at https://necolas.github.io/normalize.css/. We can apply it to our project in much the same way we did the CSS Reset. We create a file, copy the code into the file, and link it to our HTML. Once applied (Figure 1.8) we notice that our HTML looks the same as it did originally as most of the discrepancies it handles are on form elements and buttons. Depending on the browser we are using, we may notice a difference in the size of the h1.
