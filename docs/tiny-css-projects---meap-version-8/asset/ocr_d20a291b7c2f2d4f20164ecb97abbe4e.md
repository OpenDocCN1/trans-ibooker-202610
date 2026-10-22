![](bbox=[0.18, 0.124, 0.88, 0.42])

Figure 10.9 Text and Email Input Styles

# 10.3.2 Making selects and textareas match the input styles

To make sure our look and feel are consistent across our controls, let’s apply the same styles we applied to the input field to the textarea and select elements. We are not going to create new rules, or copy and paste the code. To keep our styles consistent and maintainable, we will add select and textarea as selectors to our already written rules like so:

```
input:not([type="radio"], [type="checkbox"]), textarea, select { ... }.
```

Once applied, we notice that both fields still need a little bit of extra styling. Let’s focus on the textarea first. In Figure 10.10 we can see that the textarea has the wrong typeface. The same way we inherited color earlier, we are going to also need to inherit the font-family value. Although the very rounded corners work well on the slimmer input fields, it looks a bit off on the textarea due to its height, so we are going to reduce the border-radius on just the textarea.

Figure 10.10 shows our updated text area.