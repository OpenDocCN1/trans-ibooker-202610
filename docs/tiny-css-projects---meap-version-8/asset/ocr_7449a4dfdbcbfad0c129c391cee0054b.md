# 10.3.6 Using the :checked pseudo-class

We are not going to blankety apply the same background color to our element all the time. We are going to use our accent color when the element is checked, and our hover color when it is being hovered over.

The :checked pseudo-class selector can be used on an input of type radio or checkbox, or to the option element in a drop down list (<select>) to apply styles when the element is checked. However, the ability to use it on option is browser dependent.

When we apply the background color for checked and on hover, if the selectors have the same level of specificity (our example will) the order in which we write these rules matters. If we write the checked rule first and the hover rule second, the hover color will be applied to a checked input on hover because the hover rule will override the checked rule by nature of it being later in the CSS file. We therefore want to make sure the hover rule is placed before the checked rule in the CSS file. Figure 10.18 illustrates these 2 scenarios.

![](bbox=[0.18, 0.39, 0.9, 0.67])

Figure 10.18 Rule order regarding background for checked checkboxes on hover

Let’s look at how we would go about applying our background colors in our CSS file. Listing 10.13 shows an excerpt of our file thus far.

# Listing 10.13 Inner Element Background Color

```
:where(input[type="radio"], input[type="checkbox"]):before {
  display: block;
  content: '';
  width: 0.625em;
  height: 0.625em;
}
```

13 https://developer.mozilla.org/en-US/docs/Web/CSS/:checked