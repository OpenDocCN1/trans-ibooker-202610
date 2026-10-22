# Figure 8.10 Formatted fields and error messages

# 8.2.4 Table

Next we are going to style the table. We are only going to concern ourselves with elements that relate to the theme such as colors and borders. The layout and responsiveness will come later in this chapter.

Our table is divided into three sections:

+   1. The header – `thead`
+   2. The body – `tbody`
+   3. The footer – `tfoot`

We will address each in order.

We are going to start by styling our table headers. Because they are not as important as the content found in the table itself, we give them a slightly smaller font size and lighter color than the rest of the text. We are also going to decrease their default font weight of bold to normal. By subduing them a little bit we are helping create a visual hierarchy in the table and emphasizing what the user cares the most about (the items in their shopping cart).

In our body, we are going to emphasize the item name (found in the second column) by making the text bold. To add the bold property to the item, we are going to use the pseudo class `:nth-of-type`, to specifically target the second cell of each row in the body: `tbody td:nth-of-type(2)`.

> **PSEUDO CLASS** Remember, a pseudo class matches elements based on their state. In this case their position among the same siblings (other table cells) inside of the parent container (the row).

We are also going to use `:nth-of-type`, passing in a value of `even` this time, to select the even numbered rows in the body, to create stripes. We will then add a top border to all of the rows, regardless of where they are in the table.