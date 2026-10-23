# 6

# DESIGNING PROGRAMS WITH
FUNCTIONS

![](bbox=[0.405, 0.271, 0.601, 0.407])

When writing large programs, it’s important to organize our code into smaller logical pieces, each of which contributes to the overall goal. That way, we’ll be able to think about each piece on its own, without worrying about what other pieces are doing. Then we’ll put the pieces together. These pieces are called *functions*.

In this chapter, we’ll use *functions* to break down and solve two problems: calculating the score in a two-player card game and determining whether boxes of action figures can be nicely organized.

# Problem #14: Card Game

In this problem, we’ll implement a two-player card game. As part of thinking through the problem, we’ll find that the same bit of logic crops up several times. We’ll learn how to bundle this code into a Python function to avoid code duplication and enhance code clarity.

This is DMOJ problem ccc99s1.

# *The Challenge*

Two players, A and B, are playing a card game. (You don’t need to know about playing cards or card games to understand this problem.)

The game starts with a deck of 52 cards. Player A takes a card from the deck, then player B takes a card from the deck, then player A, then