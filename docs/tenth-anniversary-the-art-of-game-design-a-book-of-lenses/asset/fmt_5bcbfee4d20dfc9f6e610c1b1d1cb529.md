
Sometimes, in the course of design, you may find a need to change one of the filters—perhaps originally you targeted one demographic (say, males ages 18–35), but while designing, you stumbled into something that better fits another demographic (say, females over 50). Changing the filters is fine, when your design constraints will allow it. The important thing is that somehow, by changing the filters or by changing your design, you find a way to get through all eight.

You will be using these filters continuously throughout the rest of the design and development process of your game. When picking an initial idea, it makes sense to evaluate which of your ideas is going to have the best shot of being molded and shaped to the point it can survive this gauntlet. The perspective of the eight filters is a very useful way to evaluate your game, so let’s make it [Lens #15](#C11_chapter8.xhtml#box15).

### [The Rule of the Loop](#C02a_toc.xhtml#rsec8_3)

It is somewhat daunting to consider that all of [Chapter 7](#C10_chapter7.xhtml#chapter7) and the first part of this one have merely been an elaboration of “(1) Think of an idea.” On the other hand, ideas are at the root of design, and their production is so mysterious as to be almost magical, so perhaps it shouldn’t surprise us that there is so much to say about this single step.

At this point in the process, you have thought of many ideas and chosen one, and now it is time to move on to the next step: “(2) Try it out.” And many designers and developers do just that—leap in and try out their game. And if your game is simple—such as a card game, board game, or very simple computer game—and you have plenty of time to test it and change it, over and over, until it is great, you probably should do just that.

But what if you can’t just build a working prototype of your game in an hour or two? What if your game vision requires months of artwork and programming before you will even be able to try it out? If this is the case (as it is for many modern videogame designs), you need to proceed cautiously at this point. The process of game design and development is necessarily iterative, or looping. It is impossible to accurately plan how many loops it is really going to take before your game passes all eight filters and is “good enough.” This is what makes game development so incredibly risky—you are gambling that you will be able to get your game to pass all eight filters on a fixed budget, when you really don’t know if that will be possible.

The naïve strategy, which many still use today, is to start slapping the game together and hope for the best. Sometimes this works. But when it doesn’t, you are in a horrible mess. You either have to ship a game that you know isn’t good enough or suffer the expense of continuing development until it is. And often, this extra time and expense is enough to make the project completely unprofitable.

In truth, this is a problem for all software projects. Software projects are so complex that it is very difficult to predict how long they will take to build and how long it will take to find and fix all of the bugs that will surely appear during development. On top of all that, games have the added burden of needing to be fun—game developers have a couple of extra filters that nongame software developers don’t need to worry about.

The real problem here is the Rule of the Loop.

> *The Rule of the Loop: The more times you test and improve your design, the better your game will be.*

The Rule of the Loop is not a lens, because it is not a perspective—it is an absolute truth. There are no exceptions to the Rule of the Loop. You will try, at times in your career, to rationalize it away, to convince yourself that “this time, the design is so good, we don’t have to test and improve,” or “we really have no choice—we’ll have to hope for the best,” and you will suffer for it each time. The horrible thing about computer games is that the amount of time and money it takes to test and adjust the system is so much greater than for traditional games. It means computer game developers have no choice but to loop fewer times, which is a terribly risky thing to do.

If you are indeed embarking on the design of a game that is likely to involve long “test and improve” loops, you need to answer these two questions:

*   **Loop question 1**: How can I make every loop count?
*   **Loop question 2**: How can I loop as fast as possible?

The software engineering people have thought about this problem a lot over the last forty years, and they have come up with some useful techniques.

### [A Short History of Software Engineering](#C02a_toc.xhtml#rsec8_4)

#### [Danger—Waterfall—Keep Back](#C02a_toc.xhtml#rsec8_4_1)

In the 1960s, when software development was still relatively new, there was very little in the way of formal process. Programmers just made their best guesses about how long things would take, and they would start coding. Often the guesses were wrong, and many software projects went disastrously over budget. In the 1970s, in an attempt to bring some order to this unpredictable process, many developers (usually at the behest of nontechnical management) tried to adopt the “waterfall model” of software development, which was an orderly seven-step process for software development. It was generally presented looking something like this:

FIGURE  

**8.2**

![](images/fig8_2.jpg)

And it certainly looks appealing! Seven orderly steps, and when each is complete, nothing remains but to move on to the next one—the very name “waterfall” implies that no iteration is needed, since waterfalls generally do not flow uphill.

The waterfall model had one good quality: it encouraged developers to spend more time in planning and design before just jumping into the code. Except for that, it is complete nonsense, because it violates the Rule of the Loop. Managers found it incredibly appealing, but programmers knew it to be absurd—software is simply too complex for such a linear process to ever work. Even Winston Royce, who wrote the paper that was the foundation for all of this, disagreed with the waterfall model as it is commonly understood. Interestingly, his original paper emphasizes the importance of iteration and the ability to go back to previous steps as needed. He never even used the word “waterfall”! But what was taught at universities and corporations everywhere was this linear approach. The whole thing seems to have been wishful thinking, mostly promulgated by people who did not actually have to build real systems themselves.

#### [Barry Boehm Loves You](#C02a_toc.xhtml#rsec8_4_2)

Then, in 1986, Barry Boehm (pronounced “beam”) presented a different model, which was based more closely on how real software development actually happens. It is usually presented as a somewhat intimidating diagram, where development starts in the middle and spirals out clockwise, passing through four quadrants again and again ([Figure 8.3](#C11_chapter8.xhtml#fig8_3)).

FIGURE  

**8.3**

![](images/fig8_3.jpg)

The spiral model of software development.

His model has a lot of complex detail, but we don’t need to go into all of that. There are basically three great ideas wrapped up in here: risk assessment, prototypes, and looping. In brief, the spiral model suggests that you do the following:

1.  Come up with a basic design.
2.  Figure out the greatest risks in your design.
3.  Build prototypes that mitigate those risks.
4.  Test the prototypes.
5.  Come up with a more detailed design based on what you have learned.
6.  Return to step 2.

And basically, you repeat this loop until the system is done. This beats the waterfall model hands down, because it is all about the Rule of the Loop. Also, it answers the questions we stated earlier:

*   **Loop question 1**: How can I make every loop count?
    **Spiral model answer**: Assess your risks and mitigate them.
