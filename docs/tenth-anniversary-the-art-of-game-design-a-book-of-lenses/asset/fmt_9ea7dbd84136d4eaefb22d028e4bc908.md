
*   Everyone loves the “dino-sub” designs. There is strong agreement between team members and potential audience members that “swimming dinosaurs” are the right look and feel for this game.
*   After several experiments, it becomes clear that for most levels, 60% of time should be spent underwater, 20% in the air, and 20% near the surface, where players who grab the right power-ups can fly above the water for a speed advantage.
*   The early networked experiments show that mostly the racing is not a problem for multiplay, but if we can avoid using rapid-fire machine guns, multiplay will be a lot easier.

### [Loop 3: “Flying Dinos” Game](#C02a_toc.xhtml#rsec8_10_3)

*   Problem statement: Design a “flying dino” game where dinosaurs race in and above the water.
*   Detailed problem statements
    *   We need to figure out if we can schedule all the animation time needed for the dinosaurs.
    *   We need to develop the “right” number of levels for this game.
    *   We need to figure out all the power-ups that will go into this game.
    *   We need to determine all the weapons that this game should support (and avoid rapid-fire machine guns because of networking constraints).

Notice how the problem statements gradually evolved and got more specific with each loop. Also notice how ugly problems bubbled to the surface quickly: What if the team hadn’t tried out all the different character designs so early? What if three levels of the game had already been designed and modeled before anyone noticed the problem of keeping players in the air for the right amount of time? What if the machine gun system had already been coded up and the whole gameplay mechanic centered around it, before anyone realized it would break the networking code? These problems got addressed quickly because of so many early loops. It looks like just two complete loops and the beginning of a third one, but because of the wise use of parallelism, there were really six design loops.

Also notice how the whole team was involved in important design decisions. There is no way that a lone designer could have done this—much of the design was informed by the technology and the aesthetics.

### [How Much Is Enough?](#C02a_toc.xhtml#rsec8_11)

> *…and now I saw, though too late, the folly of beginning a work before we count the cost, and before we judge rightly of our own strength to go through with it.*
>
> —Robinson Crusoe

You might wonder how many loops are needed before the game is done. This is a very hard question to answer, and it is what makes game development so difficult to schedule. The Rule of the Loop implies that one more loop will always make your game a little better. So as the saying goes, “the work is never finished—only abandoned.” The important thing is to make sure you get enough loops in to produce a game you are proud of before you’ve used up the entire development budget.

So when you stand there at the beginning of the first loop, is it possible to make an accurate estimate of when you will have a finished, high-quality game? No. It is simply not possible. Experienced designers, after a time, get better at guessing, but the large number of game titles that ship later than originally promised, or with lower quality than originally promised, is testament to the fact that there is just no way to know. Why is this? Because at the beginning of the first loop, you don’t yet know what you are going to build! With each loop, though, you get a more solid idea of what the game will really be, and this allows for more accurate estimates.

Game designer Mark Cerny has described a system for game design and development that he calls “The Method.” Not surprisingly, this features a system of iteration and risk mitigation. But The Method makes an interesting distinction between what Cerny calls “preproduction” and “production” (terms borrowed from Hollywood). He argues that you are in preproduction until you have finished two publishable levels of your game, complete with all necessary features. In other words, until you have two completely finished levels, you are still figuring out the fundamental design of your game. Once you reach this magic point, you are now in production. This means that you know enough about what your game really is that you can safely schedule the rest of development. Cerny states that usually this point is generally reached when 30% of the necessary budget has been spent. So if it costs you $1 million to get to this point, it will probably cost you another $2.3 million to actually complete the game. This is a great rule of thumb, and realistically, this might be the most accurate way to really plan the release date for a game. The problem with it is that you won’t really know what the game will cost or when it will be complete until you have already spent 30% of what it will take to get there. In truth, this problem is unavoidable—The Method just guides you toward reaching a point of predictability as soon as is realistically possible.

Over the years, I have developed my own rules of thumb for getting games done on time and under budget, which I call the Plan-To-Cut Rule and the 50% Rule.

**The Plan-To-Cut Rule**: When planning your game, make sure that you build it in such a way that if 50% of your budget were removed, you could still have a shippable game. This rule will force you to keep your systems simple and will help guarantee that when things go wrong (and they will go wrong) and you have to cut some features, you will still be guaranteed a shippable game.

**The 50% Rule**: All core gameplay elements should be fully playable at the halfway mark in your schedule. If this is true, it means you’ll spend about half the time getting the game working and half the time making it great. All too often, developers plan to spend 80% of the time getting it working and 20% of the time making it great. And of course, things go wrong, and that 20% gets eaten up, and in the end, you have a game that is late and weak. If you plan to have all systems playable at the 50% mark, even when things go wrong, you will have time for the important loops that make your game great.

### [Your Secret Fuel](#C02a_toc.xhtml#rsec8_12)

Much of this chapter has been very analytical, and that’s as it should be, since thoughtful analysis will do a great deal to help ensure that your game design and development are optimized. But with all that analysis, it can be easy to forget why you pursued this idea in the first place.

Now that we have discussed how games should be made, it’s time to consider who we are making them for.

### [Other Reading to Consider](#C02a_toc.xhtml#rsec8_13)

*   ***Sketching User Experiences* by Bill Buxton**. This book examines the notion of sketching (Hint: prototypes are a kind of sketch) across a wide variety of disciplines, with eye-opening results.
*   ***Have Paper, Will Prototype* by Bill Lucas**. This lecture is a series of case studies about how to successfully create paper prototypes of computer interfaces.
*   ***The Kobold Guide to Board Game Design* by Mike Selinker**. The very best book there is on how to design great board games.
*   ***Less Talk, More Rock* by Superbrothers**. This article argues that games are a medium of action, not of words, and convincingly argues that too much talking about design can be fatal.
*   ***Agile Software Development***. The Wikipedia entry about Agile Software Development is quite well written and has excellent references if you’d like to learn more about agile.
*   ***The 4Fs of Game Design: Fail Faster, and Follow the Fun* by Jason Vandenberghe**. This article (based on an idea from Marc LeBlanc) boils down the key aspects of great game design process to clear, basic elements.

# [CHAPTER NINE](#C02a_toc.xhtml#rchapter9)

## [The Game Is Made for a *Player*](#C02a_toc.xhtml#rchapter9)

FIGURE 9.1

![](images/fig9_1.jpg)
