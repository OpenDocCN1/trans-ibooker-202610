content auto min-content, while at the same time setting the height of the inner container to 100% minus the padding and margin we allotted it.

Earlier in this chapter we had given the inner container a margin of 1rem and padding of 1rem, meaning that the height it needs assigned to take up the full height of the space provided is equal to 100% minus 4rem (1rem of padding and one of margin at the top and same at the bottom equaling 4 rem total). To achieve this in CSS, we use the calc() function to do the math for us, assigning calc(100% - 4rem) to the height property.

The combination of defined rows (grid-template-rows: min-content auto min-content) and set height creates a layout where the header and link only take as much room as they need and the middle section (the paragraph element) gets what is left. Lastly, to center the paragraph content vertically in the middle of the card, we use the align-items property, with a value of center and remove the bottom margin automatically added by the browser to the h2.

If we left the margin at the bottom of the header, we would have more room at the top of the paragraph than at the bottom because min-content takes the margin included on an element into account. The link at the bottom of the card not having any margin, there would be a disproportionate amount of white space above the paragraph compared with below.

Listing 5.14 shows our layout adjustments.

# Listing 5.14 Inner container layout adjustments

```
main > section > div {
  background-color: rgba(0, 0, 0, .75);
  margin: 1rem;
  padding: 1rem;
  color: whitesmoke;
  text-align: center;
  height: calc(100% - 4rem);
  display: grid;
  grid-template-rows: min-content auto min-content;
  align-items: center;
}

section h2 {
  font-size: 1.3rem;
  font-weight: bold;
  line-height: 1.2;
  margin-bottom: 0;
}
```

This last adjustment finishes our card layout (Figure 5.16). We will now focus on showing and hiding parts of the content for devices that are wide enough (width greater than or equal to 700px) and have hover capabilities.