![](bbox=[0.3, 0.09, 0.71, 0.46])

Figure 3-18. Bidirectional filtering

I can hear those cries from the ether again, “But Jereme, if bidirectional filtering does that, why don’t we just make that the default all the time?” The short answer is that it’s not as performant, and you can create relationships where something called ambiguity can happen, which prevents other relationships from being used. Generally, the best data modeling practice would be something called a star schema, where you have a table containing your facts and then other smaller tables surrounding it like a star that filters that table.

Okay, well what about that single table that solves world hunger? Wouldn’t that be fine? Well, the short answer is no. A large single table doesn’t leverage the benefits of a dimensional model approach, which has several data governance and ease-of-use benefits. Also, a single large table can quickly become unwieldy, as you create many complex edits in Power Query to facilitate the creation of that single table. It really is easier to deal with a table with five columns and a table with ten columns compared to a single table with fifteen columns.