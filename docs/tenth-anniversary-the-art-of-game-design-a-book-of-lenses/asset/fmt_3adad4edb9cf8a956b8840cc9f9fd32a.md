
## Rule #6: Multiplication Rule for Non-Mutually Exclusive Events

This rule is almost the opposite of the previous one! If we want to find the probability of two things happening simultaneously, we can multiply their probabilities to get the answer—but ONLY if the two events are NOT mutually exclusive! Consider two die rolls. If we want to find the probability of rolling a six on both rolls, we can multiply together the probabilities of the two events: The chance of getting a six on one die roll is `1/6`, and also `1/6` for a second die roll. So the chance of getting two sixes is `1/6 × 1/6 = 1/36`. You could also have determined that by enumeration, of course, but this is a much speedier way to do it.

In Rule #5, we asked for the probability of drawing an ace OR a diamond from a deck of cards—the rule failed, because the two events were not mutually exclusive. So what if we asked about the probability of drawing an ace AND a diamond? In other words, what is the probability of drawing the ace of diamonds? It should be fairly intuitive that the answer is `1/52`, but we can check that with Rule #6, since we know the two events are not mutually exclusive. The chance of getting an ace is `4/52`, and the chance of a diamond is `13/52`. Multiplying them, `4/52 × 13/52 = 52/2704 = 1/52`. So, the rule works and matches our intuition.

Do we have enough rules yet to solve Chevalier's problems? Let's consider his first game:

**First Game:** In four rolls of a single die, the Chevalier wins if at least one six comes up.

We've already established that we could enumerate this and get the answer `671/1296`, but that would take an hour. Is there a quicker way, using the rules we have?

(I'll warn you now—this gets a little hairy. If you don't really care that much, save yourself the headache, and just skip to Rule #7. If you do care, then press on—you will find it worth the effort.)

If the question was about the chances of rolling a die four times and getting four sixes, that would be an AND question for four events that are not mutually exclusive, and we could just use Rule #6: `1/6 × 1/6 × 1/6 × 1/6 = 1/1296`. But that isn't what is asked. This is an OR question for four events that are not mutually exclusive (it is possible for the Chevalier to get multiple sixes on the four rolls). So what can we do? Well, one way is to break it down into events that are mutually exclusive and then add them up. Another way to phrase this game is:

What are the chances of rolling four dice, and getting either

1. Four sixes, OR
2. Three sixes and one non-six, OR
3. Two sixes and two non-sixes, OR
4. One six and three non-sixes

That might sound a little complicated, but it is four different mutually exclusive events, and if we can figure the probability of each, we can just add them up and get our answer. We've already figured out the probability of (a), using Rule #6: `1/1296`. So, how about (b)? Really, (b) is four different mutually exclusive possibilities:

1. 6, 6, 6, non-six
2. 6, 6, non-six, 6
3. 6, non-six, 6, 6
4. Non-six, 6, 6, 6

The probability of rolling a six is `1/6`, the probability of rolling a non-six is `5/6`. So, the probability of each of those is `1/6 × 1/6 × 1/6 × 5/6 = 5/1296`. Now, if we add up all four, that comes to `20/1296`. So, the probability of (b) is `20/1296`.

How about (c)? This one is the same as the last, but there are more combinations. It is tricky to figure out how many ways there are for exactly two sixes and two non-sixes to come up, but there are six ways:

1. 6, 6, non-six, non-six
2. 6, non-six, 6, non-six
3. 6, non-six, non-six, 6
4. non-six, 6, 6, non-six
5. non-six, 6, non-six, 6
6. non-six, non-six, 6, 6

And the probability of each of these is `1/6 × 1/6 × 5/6 × 5/6 = 25/1296`. Adding up all six of them comes to `150/1296`.

This leaves only (d), which is the inverse of (b):

1. Non-six, non-six, non-six, 6
2. Non-six, non-six, 6, non-six
3. Non-six, 6, non-six, non-six
4. 6, non-six, non-six, non-six

The probability of each is `5/6 × 5/6 × 5/6 × 1/6 = 125/1296`. Adding up all four gives `500/1296`.

So, we have now calculated the probability of the four mutually exclusive events:

1. Four sixes—(`1/1296`)
2. Three sixes and one non-six—(`20/1296`)
3. Two sixes and two non-sixes—(`150/1296`)
4. One six and three non-sixes—(`500/1296`)

Adding up those four probabilities (as Rule #5 allows) gives us a total of `671/1296`, or about `51.77%`. So, we can see that this was a good game for the Chevalier—by winning more than 50% of the time, he eventually was likely to make a profit, but the game was close enough to even that his friends believed they had a chance—at least for a while. It certainly is a very different result than the `66%` chance of winning the Chevalier believed he had!

This is the same answer we could have gotten from enumeration, but much faster. Really, though, we did a kind of enumeration—it is just that the rules of addition and multiplication let us count everything up much faster. Could we do the same thing to get the answer to Chevalier's second game? We could, but with 24 rolls of two dice, it would probably take an hour or more! This is faster than enumeration, but we can do even better by being tricky—that's where Rule #7 comes in.

## Rule #7: One Minus "Does" = "Doesn't"

This is a more intuitive rule. If the chance of something happening is `10%`, the chance of it not happening is `90%`. Why is this useful? Because often it is quite hard to figure out the chance of something happening but easy to figure out the chance of it NOT happening.

Consider Chevalier's second game. To figure out the chance of double sixes coming up at least once on twenty-four die rolls would be nightmarish to figure out, because you have so many different possible events to add together (1 double sixes, 23 non–double sixes; 2 double sixes, 22 non–double sixes; etc.). On the other hand, what if we ask a different question: What are the chances of rolling two dice twenty-four times and NOT getting double sixes? That is now an AND question, for events that are not mutually exclusive, so we can use Rule #6 to get the answer! But first we'll use Rule #7 twice—watch.

The chance of double sixes coming up on a single roll of the dice is `1/36`. So, by Rule #7, the chance of not getting double sixes is `1 − 1/36`, or `35/36`.

So, using Rule #6 (multiplication), the chances of not getting double sixes twenty-four times in a row is `35/36 × 35/36` twenty-four times, or as we say `(35/36)^(24)`. You would not want to do this calculation by hand, but using a calculator, you find the answer is around `0.5086`, or `50.86%`. But that is the chance of the Chevalier losing. To find the chance of the Chevalier winning, we apply Rule #7 again: `1 − 0.5086 = 0.4914`, or about `49.14%`. Now it is clear why he lost this game! His chances of winning were close enough to even that it was hard for him to tell if this was a winning or losing game, but after playing many times, he was very likely to lose.

Even though all probability problems can be solved through enumeration, Rule #7 can be a really handy shortcut. In fact, we could have used the same rule to solve the Chevalier's first game!

## Rule #8: The Sum of Multiple Linear Random Selections Is NOT a Linear Random Selection!

Don't panic. This one sounds hard, but it is really easy. A "linear random selection" is simply a random event where all the outcomes have an equal chance of happening. A die roll is a great example of a linear random selection. If you add up multiple die rolls, though, the possible outcomes do NOT have an equal chance of happening. If you roll two dice, for example, your chance of getting a seven is very good, while your chance of getting a twelve is small. Enumerating all the possibilities shows you why:

![](images/c012_untbl_1.jpg)
