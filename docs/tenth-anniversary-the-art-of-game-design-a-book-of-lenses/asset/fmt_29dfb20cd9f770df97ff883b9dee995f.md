
## 3. Have bounded areas that may or may not be connected

The game of tic-tac-toe, for example, features a board that is discrete and 2D. What do we mean by “discrete”? Well, even though we commonly draw a tic-tac-toe board like this:

**Figure 12.2**

![](images/fig12_2.jpg)

It is not really a continuous space, because we only care about boundaries, not the space within each cell. Whether you put your X…

**Figure 12.3**

![](images/fig12_3.jpg)

**Figure 12.4**

![](images/fig12_4.jpg)

**Figure 12.5**

![](images/fig12_5.jpg)

It doesn’t really matter—all those are equivalent in terms of the game. But if you put your X here:

**Figure 12.6**

![](images/fig12_6.jpg)

That is another matter entirely. So, even though the players can make their marks in an infinite number of places in a continuous 2D space, there are really only nine discrete places that have any actual meaning in the game. In a sense, we really have nine zero-dimensional cells, connected to each other in a 2D grid, like this:

**Figure 12.7**

![](images/fig12_7.jpg)

Each circle represents a zero-dimensional place, and each line shows which places are connected to each other. In tic-tac-toe, there is no movement from place to place, but adjacency is very important. Without adjacency, it would just be nine disconnected points. With the adjacency, it becomes a discrete 2D space, with clear boundaries—the space is three cells wide and three cells high. The space for a chessboard is similar, except that it is an 8 × 8 space.

A game with fancy aesthetics can fool you into thinking that its functional space is more complex than it really is. Consider a Monopoly board.

At first glance, you might say it is a discrete 2D space, like a chessboard, with most of the middle cells missing. But it can be more simply represented as a 1D space—a single line of forty discrete points, which connects to itself in a loop. Sure, on the game board, the corner spaces look special because they are bigger, but functionally that doesn’t matter, since each game square is a zero-dimensional space. Multiple game pieces can be in a single game square, but their relative positions within that square are meaningless.

But not all game spaces are discrete. A pool table is an example of a continuous 2D space. It has a fixed length and width, and the balls can freely move about on the table, ricocheting off of the walls or falling into the holes, which are in fixed positions. Everyone would agree that the space is continuous, but is it 2D? Since clever players can sometimes cause the balls to leave the table and hop over each other, you could certainly argue that this is really a 3D game space, and for some purposes, it is useful to think of it that way. There are no hard and fast rules for these abstract functional spaces. When designing a new game, there are times it will be useful for you to think of your space as 2D and there are times when thinking of it as 3D is more useful. The same goes for continuous vs. discrete. The purpose of stripping down a game into a functional space is so that you can more easily think about it, without the distractions of aesthetics or the real world. If you are thinking about modifying the game of soccer to a playing field with new boundaries, you will probably think about it in terms of a 2D continuous space.

**Figure 12.8**

![](images/fig12_8.jpg)

But if you are thinking about modifying the height of the goal, or changing the rules about how high the players can kick the ball, or adding hills and valleys to the field, it is useful to think of it as a continuous 3D space instead.

**Figure 12.9**

![](images/fig12_9.jpg)

There might even be times you think about a soccer field as a discrete space—breaking it up into, say, nine major areas of play, with two extra areas on the left and right representing the goals. This mode of thinking might prove useful if you are analyzing the different kinds of play that take place in different parts of the field, for example. The important thing is that you come up with abstract models of your game space that help you better understand the interrelationships of your game.

### [Nested Spaces](#C02a_toc.xhtml#rsec12_1_1)

**Figure 12.10**

![](images/fig12_10.jpg)

Many game spaces are more complex than the examples we have looked at here. Often, they feature “spaces within spaces.” Computer-based fantasy role-playing games are a good example of this. Most of them feature an “outdoor space” that is continuous and 2D. A player traveling this space sometimes encounters little icons representing towns, or caves, or castles. Players can enter these as completely separate spaces, not really connected in any way to the “outdoor space” but through the gateway icon. This is not geographically realistic, of course—but it matches our mental models of how we think about spaces—when we are indoors we think about the space inside the building we are in, with little thought to how it exactly relates to the space outside. For this reason, these “spaces within spaces” are often a great way to create a simple representation of a complex world.

### [Zero Dimensions](#C02a_toc.xhtml#rsec12_1_2)

Does every game take place in a space? Consider a game like “Twenty Questions,” where one player thinks of an object, and the other player asks “yes or no” questions trying to guess what it is. There is no game board and nothing moves—the game is just two people talking. You might argue that this game has no space. On the other hand, you might find it useful to think of the game happening in a space that looks like [Figure 12.11](#C15_chapter12.xhtml#fig12_11).

**Figure 12.11**

![](images/fig12_11.jpg)

The mind of the answerer contains the secret object. The mind of the questioner is where all the weighing of the previous answers is going on, and the conversation space between them is how they exchange information. Every game has some kind of information or “state” (as we’ll see later in Mechanic 2), and this has to exist somewhere. So, even if a game takes place in a single point of zero dimensions, it can be useful to think of it as a space. You may find that figuring out an abstract model for a game whose space seems to be trivial may lead you to insights about it that surprise you.

Being able to think about the space of your game in functional abstract terms is an essential perspective for a designer, and it is [Lens #26](#C15_chapter12.xhtml#box26).

When thinking about game spaces, it is easy to be swayed by aesthetics. There are many ways to represent your game space, and they are all good, as long as they work for you. When you can think of your space in these pure abstract terms, it helps you let go of assumptions about the real world, and it lets you focus on the kinds of gameplay interactions you would like to see. Of course, once you have manipulated the abstract space so that you are happy with its layout, you will want to apply aesthetics to it. The Lens of Functional Space works quite well with [Lens #10](#C08_chapter5.xhtml#box10): *[Holographic Design](#C08_chapter5.xhtml#box10)*. If you can simultaneously see your abstract functional space and the aesthetic space the player will experience, as well as how they interrelate, you can make confident decisions about the shape of your game’s world.

## [Mechanic 2: Time](#C02a_toc.xhtml#rsec12_2)

In the real world, **time** is the most mysterious of dimensions. Against our will, we travel through it, ever forward, with no way to stop, turn around, slow down, or speed up. In the world of games, we often try to remedy this lack of control by creating toy worlds that let us play with time like gods.

### [Discrete and Continuous Time](#C02a_toc.xhtml#rsec12_2_1)
