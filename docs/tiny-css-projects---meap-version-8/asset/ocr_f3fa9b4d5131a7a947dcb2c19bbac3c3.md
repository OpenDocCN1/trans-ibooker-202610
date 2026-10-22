With the gaps added, let’s switch our focus to adjusting our layout based on screen size.

# 2.5 Media Queries

CSS allows us to conditionally apply our styles to our layout based upon the screen size. Media queries are called at-rules because they start with the @ symbol. We then define the condition under which our styles should be applied.

Currently if we look at our layout on a wide screen (Figure 2.13), we notice we have a large amount of space in the center of the page that could be better utilized.

![](bbox=[0.18, 0.3, 0.88, 0.62])

Figure 2.13 Our layout on a wide screen

Let’s create a media query that targets screens wider than 955px. The query looks as follows: @media (min-width: 955px) { }. All of the rules we place inside of the curly brackets ({}) will only be applied if the screen size is greater than or equal to 955px. These styles will override the styles we had already assigned to the grid since by being inside of the media query, they have a higher level of specificity then our original layout.

Listing 2.8 shows our media query. After defining the query, we redefine our grid-template-areas to have a different configuration. We also update the column sizes to all have equal widths.