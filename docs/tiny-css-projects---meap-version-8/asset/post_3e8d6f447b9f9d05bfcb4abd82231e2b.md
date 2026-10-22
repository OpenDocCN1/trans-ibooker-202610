
# Newspaper Title

Tuesday, 5th September 2021

# Article heading

John Doe

Maecenas faucibus mollis interdum. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Cras justo odio, dapibus ac facilisis in, egestas eget quam. Sed posuere consectetur est at lobortis. Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Curabitur blandit tempus porttitor.

Integer posuere erat a ante venenatis dapibus posuere velit aliquet. Maecenas faucibus mollis interdum. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Vivamus sagittis lacus vel augue laoreet rutrum faucibus dolor auctor. Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum.

Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa justo sit amet risus.

Aenean lacinia bibendum nulla sed consectetur. Duis mollis, est non commodo luctus, nisi erat porttitor ligula, eget lacinia odio sem nec elit. Donec id elit non mi porta gravida at eget metus. Cras justo odio, dapibus ac facilisis in, egestas eget quam. Cras mattis consectetur purus sit amet fermentum. Nullam id dolor id nibh ultricies vehicula ut id elit. Cras mattis consectetur purus sit amet fermentum.

# Subheading

- List item 1
- List item 2
- List item 3

Cras justo odio, dapibus ac facilisis in, egestas eget quam. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent commodo cursus magna, vel scelerisque nisl consectetur et. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Aenean lacinia bibendum nulla sed consectetur.

Donec ullamcorper nulla non metus auctor fringilla. Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Aenean lacinia bibendum nulla sed consectetur. Aenean lacinia bibendum nulla sed consectetur.

# Subheading

![](img/b56145aa9b751926a7aedbd7172bc818_99_0.png)

Praesent commodo cursus magna, vel scelerisque nisl consectetur et. Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Aenean lacinia bibendum nulla sed consectetur. Integer posuere erat a ante venenatis dapibus posuere velit aliquet. Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Integer posuere erat a ante venenatis dapibus posuere velit aliquet.

Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Curabitur blandit tempus porttitor. Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Duis mollis, est non commodo luctus, nisi erat porttitor ligula, eget lacinia odio sem nec elit.

Figure 4.2 Starting point

Before we worry about layout, let’s define our theme.

# 4.1 Setting up our theme

The theme sets the tone for the page and generally consists of colors, fonts, borders, and sometimes padding. Our theme will stay the same regardless of screen size or layout. Oftentimes, the theme of a website is tightly coupled to its logo and brand colors.

We will set some defaults on the <body> tag that can be inherited by its descendants. As a general rule, styles that revolve around typography, (color, font-family...) can be inherited by most elements (exceptions are some form elements, which we cover in Chapter 10). It is

worth mentioning that font-size is not inherited. By setting inheritable properties on the parent, the styles will trickle down to the descendents, preventing us from needing to apply them to every element.

# 4.1.1 Importing fonts from Google Fonts

We apply a background color, font, and text color (Listing 4.2). Notice that before the body rule, we import our chosen font-family from Google Fonts. We tend to use Google Fonts as it’s freely available, without needing to create an account or pay to use the fonts. PT Serif is not a font we can safely expect a user to already have loaded on their computer, we therefore have to import it for the browser to know what the glyphs (letters, numbers, and symbols) should look like. We also provide a default of serif as a fallback should the import fail.

# Web safe fonts

There are very few web safe fonts (fonts we can assume most devices will have access to). The generally accepted list is: Arial, Verdana, Helvetica, Tahoma, Trebuchet MS, Times New Roman, Georgia, Garamond, Courier New, Brush Script MT¹.

Regardless of the font family we choose, it is good practice to always provide a fallback value (Serif, Sans-serif, Monospace, Cursive, or Fantasy)

Although we will do the bulk of the layout later in the chapter, we also add some left and right padding on our body now to move our text away from the edge.

# Listing 4.2 Defining some theme styles

```
@import url('https://fonts.googleapis.com/css2?family=PT+Serif&display=swap');  #A

body {
  background-color: #f9f7f1;
  font-family: 'PT Serif', serif;  #B
  color: #404040;
  padding: 0 24px;
}
```

#A Importing PT Serif from Google Fonts

#B Applying PT serif to our content and providing a fallback

Figure 4.3 shows our updated page. We notice that all of the elements in the body have inherited the color and font-family.

# Newspaper Title

Tuesday, 5th September 2021

# Article heading

John Doe

Maecenas faucibus mollis interdum. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Cras justo odio, dapibus ac facilisis in, egestas eget quam. Sed posuere consectetur est at lobortis. Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Curabitur blandit tempus porttitor.

Integer posuere erat a ante venenatis dapibus posuere velit aliquet. Maecenas faucibus mollis interdum. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Vivamus sagittis lacus vel augue laoreet rutrum faucibus dolor auctor. Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum.

> "Fusce dapibus, tellus ac cursus commodo, tortor mauris condimentum nibh, ut fermentum massa justo sit amet risus."

Aenean lacinia bibendum nulla sed consectetur. Duis mollis, est non commodo luctus, nisi erat porttitor ligula, eget lacinia odio sem nec elit. Donec id elit non mi porta gravida at eget metus. Cras justo odio, dapibus ac facilisis in, egestas eget quam. Cras mattis consectetur purus sit amet fermentum. Nullam id dolor id nibh ultricies vehicula ut id elit. Cras mattis consectetur purus sit amet fermentum.

# Subheading

- List item 1
- List item 2
- List item 3

Cras justo odio, dapibus ac facilisis in, egestas eget quam. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Praesent commodo cursus magna, vel scelerisque nisl consectetur et. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Aenean lacinia bibendum nulla sed consectetur.

Donec ullamcorper nulla non metus auctor fringilla. Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Aenean lacinia bibendum nulla sed consectetur. Aenean lacinia bibendum nulla sed consectetur.

# Subheading

![](img/b56145aa9b751926a7aedbd7172bc818_101_0.png)

Praesent commodo cursus magna, vel scelerisque nisl consectetur et. Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Donec id elit non mi porta gravida at eget metus. Aenean lacinia bibendum nulla sed consectetur. Integer posuere erat a ante venenatis dapibus posuere velit aliquet. Aenean eu leo quam. Pellentesque ornare sem lacinia quam venenatis vestibulum. Integer posuere erat a ante venenatis dapibus posuere velit aliquet.

Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Curabitur blandit tempus porttitor. Morbi leo risus, porta ac consectetur ac, vestibulum at eros. Duis mollis, est non commodo luctus, nisi erat porttitor ligula, eget lacinia odio sem nec elit.

# Figure 4.3 Theme styles applied to the body being inherited by descendents

Next we will style the title, headings, and subheadings. Let’s start with the newspaper title which is the <h1> in the HTML. We want to change the font-family to use a typeface called "Oswald", increase the text size, make it bold, transform the font to use all capital letters, set the line height, and center the text. Like PT Serif, Osward is not a standard font we can expect most users’ devices to know about. We will import it much like we did PT Serif.

To import "Osward" from Google Fonts we can either add a second @import at the top of our file or for better performance we can combine the two imports into one import statement. The ability to combine the two imports is specific to Google Fonts. Not all Content Delivery Networks (CDN) will provide this ability.

Notice in Listing 4.3 that in our import after the name of the font we see :wght@400;700. This indicates which Oswald font weights we want to import.
