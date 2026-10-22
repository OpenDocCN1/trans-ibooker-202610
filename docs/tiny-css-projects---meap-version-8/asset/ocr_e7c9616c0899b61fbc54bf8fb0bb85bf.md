The edits to our base style and extras added specifically to textarea are found below in Listing 10.8.

# Listing 10.8 Updated Styles for Textarea

```
input:not([type="radio"], [type="checkbox"]),
textarea,
select {
  background: var(--background);
  font-size: 1rem;
  font-family: inherit;
  color: inherit;
  border: solid 1px var(--border);
  border-radius: 32px;
  padding: 1rem 2rem;

  display: block;
  width: calc(100% - 4rem);
}

textarea {
  border-radius: 4px;
  resize: vertical;
}
```

#A Added textarea and select to our styles
#B Make the controls inherit font size front the parent container
#C Added rule specific to textarea for border radius and resizing

We still need to address the select, but this will be a bit more involved then the edits we made for textarea, so let’s finish styling the input fields first and then we will circle back and finish styling the select control.

# 10.3.3 Styling radio inputs and checkboxes

Earlier in the chapter we talked a little bit about how some form controls are notoriously difficult to style because our options for styles that can be applied to them are incredibly limited. Radios and checkboxes fall squarely into that category. Until recently there were no properties that affect the radio circle or checkbox square whatsoever. Our only option was to completely replace the native control styles with our own.

We now have the ability to change the native control’s color. The accent-color property allows us to replace the user agent’s chosen color with the color we specify. Applying accent-color: var(--accent); to our checkbox and radios will yield the following results (Figure 10.13).