# 3.5 Accessibility and the reduced motion query

The use of motion, parallax (where the background moves slower than the foreground) and animations on the web has increased as it has become easier to implement and browser support has improved. By using these techniques we can create richer user interfaces that are interactive and provide greater depth in our experiences.

However, the use of these techniques come at a cost. For some users, especially individuals with vestibular disorders, movement on the screen can cause headaches, dizziness, and nausea. As we mentioned earlier, animations can also cause seizures, especially if they contain elements that flash.

In many operating systems users can choose to disable animations on their device. In our applications, we need to make sure that we respect those preferences. To check user settings the level 5 media queries module has introduced the prefers-reduced-motion media query. This is an at-rule, which checks the user’s preferences regarding motion on the screen and allows us to apply conditional styles based on the user’s preferences. There are two values for this query:

+   - `no-preference`
+   - `reduce`

We can either choose to disable an animation when a user prefers reduced motion, or to enable it when they have not specified a preference. The use of reduced motion by a user does not mean we cannot use any animation at all, but that we should be selective of which animations we should keep. Aspects that may determine which animations are kept enabled include:

+   - how fast it is
+   - how much of the viewport it uses
+   - flash rate
+   - how essential it is to the functioning of the site or understanding of the content

It is worth mentioning that a user may prefer reduced or no animations but may not be aware of the system preference settings to opt out of animations so providing an on site opt out button may be useful depending on how much animation our website has.

> **Accessibility Guidelines for Animations**
> A user should be able to pause, stop, or hide animation that lasts more than 3 seconds which are not considered essential. Loaders are a bit tricky when it comes to this as they convey important information to the user (the application is doing something, it is not frozen).²

Our loader could be considered essential content but we also provide a progress bar below it which also gives the user an indication of what the application is doing. Because the information is also being conveyed below using a different media, and the animation is very