When we look at the content flow we notice that the caption and the image have been split across 2 different columns which isn’t ideal. Let’s prevent that from happening.

# 4.4.6 Controlling content breaks

To prevent the image and its caption from ending up in different columns we can use the `break-inside` property with the keyword value `avoid` which we set on the `<figure>` element. With this declaration we inform the browser that when it’s generating the columns the contents of the element should stay together as a unit and should not be split across multiple columns. In other words, the image and figcaption should remain together. The rule we add to the media query is: `figure { break-inside: avoid }`. Figure 4.23 shows the resulting output.

![](bbox=[0.18, 0.35, 0.88, 0.53])

Figure 4.23 Keeping the image and caption together

# 4.5 Finishing touches

With our content flowing the way we want it across the columns let’s polish up some final details. One of the hallmarks of newspaper layouts is that the text is often justified.

# 4.5.1 Text justification and hyphens

Justification refers to the alignment of the lines inside of a body of text as described in Figure 4.24. The text being justified means that the lines of text will start and end at the same spot forming a box rather than having ragged ends like when the text is left aligned.