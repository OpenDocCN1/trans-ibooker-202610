to *all* instances of a particular tag. We usually want a more fine-grained approach to selecting what elements we style, and this is where *id selectors* and *class selectors* come into play.

# ID Selectors

The *id selector* is used to map to a unique element on your webpage. Each element on your webpage can be assigned a unique id via the `id` attribute, and it is this identifier that CSS uses to latch styles onto your element. This type of selector begins with a hash symbol (#), followed directly by the identifier of the element you wish to match to. Check out the figure below for an example.

![](bbox=[0.125, 0.38, 0.875, 0.69])

An illustration demonstrating the use of an id selector in CSS. Note the blue header has an identifier which matches the CSS attribute #blue_header.

# Class Selectors

The alternative option is to use *class selectors*. This approach is similar to that of *id selectors*, with the difference that you can legitimately target multiple elements with the same class. If you have a group of HTML elements that you wish to apply the same style, use a class-based approach. The selector for using this method is