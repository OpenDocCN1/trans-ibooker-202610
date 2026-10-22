(lang) attribute on the html tag. The value of en-US specifies that our document is written in American English, therefore the symbols that the browser renders are " and " as we can see in Figure 4.9.

# Listing 4.7 Styling the bloquote

```
blockquote {
  font: 1.8rem/1.25 'Oswald', sans-serif;
  margin: 1.5rem 2rem;
}
blockquote::before { content: open-quote; }
blockquote::after { content: close-quote; }
```

With our quote now styled, let's turn our attention to the bulleted list found in the middle of the article.

![](bbox=[0.18, 0.36, 0.88, 0.51])

Figure 4.9 Styled title, heading, subheadings, and blockquote

# 4.2 CSS counters

Our article contains an unordered (bulleted) list. Currently, each list item has the default bullet before each list item. We can alter what our bullet looks like by using the list-style-type property. By default, we can choose from disc (•), circle (○), square (■), and numbers or letters in a number of different languages, alphabets, and number formats. But let's say we wanted our bullet to be an emoji, specifically the hot beverage emoji (☕). We would have to create a custom list style.

To create our custom list type, we will use the counter-style at-rule. We have used at-rules before in chapter 3 when we created keyframes. In this case, instead of defining how an animation will behave, we will define how our list looks and behaves. The at-rule is called counter style because it specifically addresses the built counting mechanism for list items in CSS. Under the covers regardless of the list being ordered or unordered, the browser keeps track of the position of the item in the list. It counts the items.

Like with keyframes (which we named in order to reference them inside of our animation property) we will name our counter-style so that we can reference it using the list-style property and apply it to our list. Let's name our list-style "emoji". Our at-rule will therefore look as follows: @counter-style emoji { }. Next, we will define the behavior our list-style