have to check these next two cards, hoping there is no high card among them. The next two cards are not high cards—they are a three and a seven—so player A gets 2 points.

Player B now takes the second card, which is a three. Three isn’t a high card, so no points for player B.

Player A now takes the seven. No points.

Player B now takes the king, so there’s a chance for 3 points for player B. There are at least three cards remaining in the deck after this king. We have to check these next three cards, hoping there is no high card among them. Sadly, there is a high card, a jack, among those three. No points for player B.

Player A now takes the nine. No points.

Player B now takes the first jack. There is at least one card remaining in the deck after this jack. We have to check this next card, hoping it isn’t a high card. Good news: it’s not a high card—it’s an eight—so player B gets 1 point.

There’s only one more point scored, and it’s by player A when they take the second-last card (the jack) from the deck.

Therefore, this is the output for this test case:

+   Player A scores 2 point(s).
+   Player B scores 1 point(s).
+   Player A scores 1 point(s).
+   Player A: 3 point(s).
+   Player B: 1 point(s).

Notice that each time a player takes a high card, we need to check two things: that there are at least a certain number of cards remaining in the deck and that there is no high card among these cards. The first we should be able to manage by using a variable that tells us how many cards have been taken. The second is more difficult. We’d need some code to check a given number of cards for a high card. Worse, if we’re not careful, we’d end up duplicating very similar code four times: once to check the card after a jack, once to check the two cards after a queen, once to check the three cards after a king, and once to check the four cards after an ace. If we later found a flaw in our logic, we would have to fix it in up to four different places.

Is there a Python feature that lets us package that “no high cards here” logic, just once, and invoke it four times? There is. It’s called a function, and it’s just a named block of code that carries out a small task.