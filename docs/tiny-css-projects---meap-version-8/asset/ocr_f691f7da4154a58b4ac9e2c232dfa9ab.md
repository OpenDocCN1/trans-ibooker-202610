# Pseudo-Class vs Pseudo-Element

A pseudo-class is added to a selector to target a specific state while a pseudo-elements allows us to select a part of the element.

With the letter selected, we can now start styling it in order to make it look like a drop cap. To really make it stand out from the rest of the text, we are going to pick a more ornate typeface. In this particular case, we are going to use Passions Conflict (https://fonts.google.com/specimen/Passions+Conflict) seen in Figure 7.4, which we will import from google fonts.

![](bbox=[0.18, 0.31, 0.92, 0.52])

Figure 7.4 Passions Conflict Glyphs

Because this typeface has particularly ornate capital letters, it will be well suited to use for our drop cap. We will also use it a little bit later on in this chapter to style the quote at the beginning of the chapter. Beautiful typefaces such as this one are a wonderful way of embellishing a page but only for very short bits of content. Handwriting and display fonts can be quite difficult to read and are therefore not well suited for large bodies of text. For a drop cap, large headers, or short quotes, however, they can be a great way to differentiate the element from the rest of the content and give the page some personality.

This particular font has glyphs that are quite a bit smaller than those of Times New Roman (the font we use for the rest of our content), and we are creating a drop cap which by definition is larger than the rest of the text, so we are going to have to adjust its size. We are also going to adjust the line height of the letter to make it fit nicely with the text. Finally we are going to float it to the left so that the text flows around the letter, accomplishing our desired effect.

The float property places an element to the right or left of its container based upon the value it is passed. Inline elements around it (our text) will use the leftover space to wrap around the floated element. The float property can take one of three properties:

+   - Left
+   - Right
+   - None (element is not floated)