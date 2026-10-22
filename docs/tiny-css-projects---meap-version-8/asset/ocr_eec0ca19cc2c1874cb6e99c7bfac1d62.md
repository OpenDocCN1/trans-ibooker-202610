# Listing 6.9 Styling the name

```
h1 {
  font-size: 2rem;      #A
  margin: 36px 0 0;      #A
  color: var(--primary); #A
}

.title {
  font-size: 1.25rem;    #B
  font-weight: bold;     #B
  color: var(--secondary); #B
}
```

#A Styles for the name
#B Styles for the job title

![](bbox=[0.18, 0.36, 0.9, 0.52])

Figure 6.19 Styled Name and Job title

Next we are going to style the post, like, and follower information.

# 6.6.2 Space-around and gap

In our HTML, the description list (dl) contains the post, like, and follower counts (Listing 6.10). Each grouping is contained within a div, so we will apply a display value of flex to the definition list in order to horizontally align all three groups. We will then set the justify-content property to space-around in order to spread them out across the card.