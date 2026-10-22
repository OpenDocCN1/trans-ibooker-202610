Since we only want our content placed in columns on wider screens, we will use a media query to conditionally apply our columns only after the window reaches a particular size.

# 4.4.1 Media queries

Media queries are a type of at-rule. Like @counter-style which we used earlier in this chapter, it will start with an @ symbol followed by the identifier media. Then we set the instruction for when the rules found inside of the media query should apply. We will want to place the content in columns once our window width is greater than or equal to 955px. Our media query will therefore be: @media (min-width: 955px) {}. The diagram found in Figure 4.18 breaks down the individual pieces of the query.

![](bbox=[0.25, 0.33, 0.8, 0.5])

Figure 4.18 Media query breakdown

Inside of the media query we will now define our columns.

# 4.4.2 Defining and styling columns

There are 2 ways we can define how the columns are created:

+   - We can dictate a column width, and the browser will create as many columns of that width it can in the available space.
+   - We can dictate how many columns we want, and the browser will fit that number of equally sized columns in the available space.

We will go with the second option since we already know we want to create 3 columns. We therefore specifically target the article and using the column-count property set our quantity to 3 as shown in Listing 4.11.

```
Listing 4.11 Conditionally break article into 3 columns based on screen width

@media(min-width: 955px) { #A
  article {
    column-count: 3;    #B
  }
}
```