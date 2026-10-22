Figure 10.27 shows our styled select.

![](bbox=[0.18, 0.15, 0.85, 0.25])

Figure 10.27 Styled and Positioned Select Arrow

To style the select options themselves, only `color` and `background` can be styles, and even then, only some browsers will allow those to be applied. This is because the styles for options are controlled by the underlying operating system. For cross browser compatibility and accessibility, we will therefore leave those as they are.

We have now styled all of the form controls in our form. Let’s revisit our labels and add some breathing room between our labels and our controls so that our elements aren’t so closely compacted against each other.

# 10.5 Styling labels and legends

To style our labels and the legend, we are going to start by giving them `margin`. We will give them vertical margin for breathing room in between items. We will also give them left margin so that the label text aligns with where the text begins in the input field. We gave the inputs left padding of `2rem`. We can therefore give our labels the same amount of left margin to align the text visually on the page. Because labels have a `display` value of `inline` by default, we will need to give them a `display` value of `block` for the margin to apply. Finally we will also decrease their font size. The information that is most important here is the values that are entered by the user, not the labels. By decreasing their size we diminish their importance in the visual hierarchy. We end up with the code displayed in listing 10.19.

# Listing 10.19 Added margin and updated font size

```
label, legend { #A
  display: block;
  margin: 1rem 0 0 2rem; #B
  font-size: .875rem;
}
```

#A we apply the styles to both the labels and the legend
#B Margin top of 1 rem, margin left of 2 rem

Notice in Figure 10.26 that the `legend` is still very close to the "Reason for Contact" field above it. Also, our radio buttons are now very far apart.