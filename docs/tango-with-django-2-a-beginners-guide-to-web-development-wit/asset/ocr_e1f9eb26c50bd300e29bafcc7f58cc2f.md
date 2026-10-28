![](bbox=[0.15, 0.127, 0.847, 0.46])

**The result of the code we demonstrate above – a small popup box that displays `Hello, world!` when the page is loaded. This should be visible on every page of Rango.**

You should, of course, remove the `alert()` function call once you have seen it working in front of your very eyes – this is perhaps the simplest demonstration we can implement. Being greeted with this popup on every page is very annoying!

# Select and Act Pattern

JQuery requires you to think in a more *functional* programming style, as opposed to typical JavaScript which is written in a more *procedural* way. For all JQuery commands, they follow a similar pattern: **Select and Act.** Select an element, and then perform some kind of action on/with the element.

# Acting on Mouse Clicks

In the previous example, we executed the `alert()` function call when the page had been loaded, and everything was ready to go. This is based upon an *event* being