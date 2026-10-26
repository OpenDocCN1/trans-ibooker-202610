Perform the following steps to solve the activity:

+   1. First, determine which container would be the most suitable to store the cards of each player. We should have four containers that have a set of cards – one for each player.
+   2. Write a function to initialize and shuffle the cards.
+   3. Write a function to randomly deal all the cards among the four players.
+   4. Write a matching function. This function will pick a card from each player and compare it as required by the rules of the game. Then, it will remove the necessary cards. We have to choose the card wisely so that removing it would be faster. This parameter should also be considered while deciding on the container.
+   5. Now, let's write a function, to see whether we have a winner.
+   6. Finally, we'll write the core logic of the game. This will simply call the matching function until we have a winner based on the function written in the previous step.

> **Note**
> 
> The solution to this activity can be found on page 482.

# std::deque – Special Version of std::vector

So far, we have seen array-based and linked list-based containers. **std::deque** mixes both of them and combines each of their advantages to a certain extent. As we have seen, although vector is a variable-length array, some of its functions, such as **push_front** and **pop_front**, are very costly operations. **std::deque** can help us overcome that. Deque is short for double-ended queue.

# The Structure of Deque

The C++ standard only defines the behavior of the containers and not the implementation. The containers we have seen so far are simple enough for us to predict their implementation. However, deque is slightly more complicated than that. Therefore, we'll first take a look at its requirements, and then we will try to dive into a little bit of implementation.