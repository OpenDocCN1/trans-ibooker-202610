interacted with a field, also sometimes referred to as the field being "dirty". This is where we need a little help from JavaScript. The script attached to the file listens for on blur events. Blur events happen when an element loses focus. When we click or tab away from a field, an blur event occurs. Our script listens for these blur events and adds a class of dirty to the field which we have just navigated away from. This allows us to know which fields have been interacted with versus not. Those with a class of dirty have, those who do not have a class of dirty have not. When the "Clear" button is clicked, it clears the form and removes the class of dirty from any of the fields that may have had it, resetting the form to a clean state.

Since we have this dirty class, in conjunction with the :invalid attribute, we will only show the error message below controls that are not only invalid, but have also been touched by the user, preventing us from showing error messages before the user has a chance to fill out the form. We use the following selector: .dirty:invalid + .error span. We select the span contained in an element that has a class of error that is located immediately after an element that is both invalid and has a class of dirty.

Lastly we will also change the border color of the field to our error color when it is both invalid and dirty.

Listing 10.27 shows the full rules to show and hide the error messages.

# Listing 10.27 Error Handling CSS

```
.error span { display: none; } #A

.dirty:invalid + .error span { #B
  display: inline;      #B
}                       #B

:is(input, textarea).dirty:invalid { #C
  border-color: var(--error);  #C
}                           #C
```

#A Hide the error message by default
#B Show the error message when the field immediately before it in the HTML is dirty and invalid
#C Change input and textarea border color to red when invalid and dirty

Figure 10.35 shows fields in their 3 possible states, invalid and dirty, valid, and invalid but not yet touched.