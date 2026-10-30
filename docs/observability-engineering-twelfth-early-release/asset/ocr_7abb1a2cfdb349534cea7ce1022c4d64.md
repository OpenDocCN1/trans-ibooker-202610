I want to pause here to stress that optimizing for development speed was *the right thing to do*. With that decision, we made many early choices that we later had to undo and redo. But most start-ups don’t fail because they make the wrong tooling choices. And let’s be clear: most start-ups do fail. They fail because there’s no demand for their product, or because they can’t find product/market fit, or because customers don’t love what they built, or any number of reasons where time is of the essence. Choosing a stack that used MongoDB and Ruby on Rails enabled us to get to market quickly enough that we delighted customers, and they wanted a lot more of what we were selling.

Around the time Facebook acquired Parse, we were hosting over 60,000 mobile apps. Two and a half years later, when I left Facebook, we were hosting over a million mobile apps. But even when I first joined, in 2012, the cracks were already starting to show.

Parse officially launched a couple of months after I joined. Our traffic doubled, then doubled, and then doubled again. We were the darling of Hacker News, and every time a post about us showed up there, we’d get a spike of new sign-ups.

In August of 2012, one of our hosted apps moved into a top 10 spot in the iTunes Store for the first time. The app was marketing a death-metal band from Norway. The band used the app to livestream broadcasts. For the band, this was in the evening; for Parse, it was the crack of dawn. Every time the band livestreamed, Parse went down in seconds flat. We had a scaling problem.

# Scaling at Parse

At first, figuring out what was happening to our services was difficult. Our diagnosis was challenging because (thanks to our co-tenancy model) whenever something got slow, *everything* got slow, even if it had nothing to do with whatever was causing the problem.