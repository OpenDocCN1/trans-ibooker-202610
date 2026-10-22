![](bbox=[0.18, 0.13, 0.9, 0.45])

Figure 10.28 Added Label and Legend Margin

To add space between our legend and the "Reason for Contact" above we are going to give our fieldset itself some margin. We will then specifically target the labels that are within the fieldset and reduce their vertical margin so that the radio inputs are closer together. We want to keep the radio inputs close together because the closer they are to one another visually, the easier it will be for the user to recognize them as belonging together as a group. Listing 10.20 shows our updated margins.

# Listing 10.20 Added margin and updated font size

```
fieldset {
    #A
    ...
    #A
    margin: 1rem 0;
    #A
}

fieldset label {
    #B
    margin: .5rem 0 0 2rem;
    #B
```

#A Add margin to our already existing fieldset rule
#B Update the margin of the labels that inside of the fieldset

With our labels now formatted (Figure 10.29) let’s continue down the form and style the buttons.