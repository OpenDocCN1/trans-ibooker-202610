
# What are the real odds of Chevalier's games?

To understand that, we have to get into some math—don't fret, it's easy math that anyone can do. Fully covering the mathematics of probability is not necessary for game design (and beyond the scope of this book), but knowing some of the basics can be quite handy. If you are a math genius, you can skip this section, or at least read it smugly. For the rest of us, I present the following:

## Ten Rules of Probability Every Game Designer Should Know [`#C02a_toc.xhtml#rsec12_7_2`]

### Rule #1: Fractions Are Decimals Are Percents [`#C02a_toc.xhtml#rsec12_7_2_1`]

If you are one of those people who has always had a hard time with fractions and percents, it's time to face up and deal with them, because they are the language of probability. Don't stress—you can always use a calculator—no one is looking. The thing you have to come to grips with is that fractions, decimals, and percents are all the same thing and can be used interchangeably. In other words, ½ = 0.5 = 50%. Those aren't three different numbers; they are just three ways of writing exactly the same number.

Converting from fractions to decimals is easy. Need to know the decimal equivalent of 33/50? Just type 33 ÷ 50 into your calculator, and you'll get 0.66. What about percents? They're easy too. If you look up the word "percent" in the dictionary, you'll see that it really means "per 100." So, 66% really means 66 per 100, or 66/100, or 0.66. If you look at Chevalier's previous math, you'll see why we need to convert back and forth so often—as humans, we like to talk in percents, but we also like to talk about "one chance in six"—so we need a way to convert between these forms. If you are the kind of person who suffers from math anxiety, just relax and practice a few of these on the calculator—you'll have the hang of it in no time.

### Rule #2: Zero to One—and That's It! [`#C02a_toc.xhtml#rsec12_7_2_2`]

This one's easy. Probabilities can only range from 0% to 100%, that is, from 0 to 1 (see Rule #1), no less and no more. While you can say there is a 10% chance of something happening, there is no such thing as a −10% chance and certainly no such thing as a 110% chance. A 0% chance of something happening means it won't happen, and a 100% chance means it definitely will. This all might sound obvious, but it points out a major problem with Chevalier's math. Consider his first game with the four dice. He believed that with four dice, he had a 4 × (1/6), or 4/6, or 0.66, or 66% chance of having a six come up. But what if he had seven dice? Then he would have had 7 × (1/6) or 7/6 or 1.17 or 117% chance of winning! And that is certainly wrong—if you roll a die seven times, it might be likely that a six will come up one of those times, but it is not guaranteed (in fact, it is about a 72% chance). Anytime you calculate a probability that comes up greater than 100% (or less than 0%), you know for certain that you've done something wrong.

### Rule #3: "Looked For" Divided By "Possible Outcomes" Equals Probability [`#C02a_toc.xhtml#rsec12_7_2_3`]

The first two rules lay some basic groundwork, but now we are going to talk about what probability really is—and it is quite simple. You just take the number of times your "looked for" outcome can come up and divide by the number of possible outcomes (assuming your outcomes are equally likely), and you've got it. What is the chance of a six coming up when you roll a die? Well, there are six possible outcomes, and only one of them is the one we are looking for, so the chance of a six coming up is 1 ÷ 6, or 1/6, or about 17%. What is the chance of an even number coming up when you roll a die? There are 3 even numbers, so the answer is 3/6, or 50%. What is the chance of drawing a face card from a deck of cards? There are twelve face cards in a deck, and fifty-two cards total, so your chances of getting a face card are 12/52, or about 23%. If you understand this, you've got the fundamental idea of probability.

### Rule #4: Enumerate! [`#C02a_toc.xhtml#rsec12_7_2_4`]

If Rule #3 is as simple as it sounds (and it is), you might wonder why probability is so tricky. The reason is that the two numbers we need (the number of "looked for" outcomes and the number of possible outcomes) are not always so obvious. For example, if I asked you what the odds of flipping a coin three times and getting "heads" at least twice, what is the number of "looked for" outcomes? I'd be surprised if you could answer that without writing anything down. An easy way to find out the answer is to enumerate all the possible outcomes:

1. HHH
2. HHT
3. HTH
4. HTT
5. THH
6. THT
7. TTH
8. TTT

There are exactly eight possible outcomes. Which ones have heads at least twice? #1, #2, #3, and #5. That's 4 outcomes out of 8 possibilities, so the answer is 4/8, or a 50% chance. Now, why didn't the Chevalier do this with his games? With his first game, there were four die rolls, which means 6 × 6 × 6 × 6, or 1296 possibilities. It would have been dull work, but he could have enumerated all the possibilities in an hour or so (the list would have looked like 1111, 1112, 1113, 1114, 1115, 1116, 1121, 1122, 1123, etc.), then counted up the number of combinations that had a six in them (671), and divided that by 1296 for his answer. Enumeration will let you solve almost any probability problem, if you have the time. Consider the Chevalier's second game, though: 24 rolls of 2 dice! There are 36 possible outcomes for 2 dice, and so enumerating all 24 rolls would have meant writing down 36^(24) (a number 37 digits long) combinations. Even if he could somehow write down one combination a second, it would have taken longer than the age of the universe to list them all. Enumeration is handy, but when it takes too long, you need to take shortcuts—and that's what the other rules are for.

### Rule #5: In Certain Cases, OR Means Add [`#C02a_toc.xhtml#rsec12_7_2_5`]

Very often, we want to determine the chances of "this OR that" happening, such as what are the chances of drawing a face card OR an ace from a deck of cards? When the two things we are talking about are mutually exclusive, that is, when it is impossible for both of them to happen simultaneously, you can add their individual probabilities to get an overall probability. For example, the chances of drawing a face card are 12/52, and the chances of drawing an ace are 4/52. Since these are mutually exclusive events (it is impossible for them both to happen at once), we can add them up: 12/52 + 4/52 = 16/52, or about a 31% chance.

But what if we asked a different question: What are the chances of drawing an ace from a deck of cards or a diamond? If we add these probabilities, we get 4/52 + 13/52 (13 diamonds in a deck) = 17/52. But, if we enumerate, we see this is wrong—the right answer is 16/52. Why? Because the two cases are not mutually exclusive—I could draw the ace of diamonds! Since this case is not mutually exclusive, "or" does not mean add.

Let's look at Chevalier's first game. He seems to be trying to use this rule for his die rolls—adding up four probabilities: 1/6 + 1/6 + 1/6 + 1/6. But he gets the wrong answer, because the four events are not mutually exclusive. The addition rule is handy, but you must be certain the events you are adding up are mutually exclusive from one another.

### Rule #6: In Certain Cases, AND Means Multiply [`#C02a_toc.xhtml#rsec12_7_2_6`]
