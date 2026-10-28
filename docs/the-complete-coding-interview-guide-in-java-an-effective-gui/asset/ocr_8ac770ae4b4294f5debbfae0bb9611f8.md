```
this.cards = pack.getCards();
}

public void shuffle() {...}
public List<T> dealHand(int numberOfCards) {...}
public T dealCard() {...}
public int remainingCards() {...}
public void removeCards(List<T> cards) {...}

@Override
public Iterator<T> iterator() {...}
}
```

A demo of the code can be quickly written as follows:

```
// create a single classical card
Card sevenHeart = new StandardCard(StandardSuit.HEARTS, 7);

// create a complete deck of standards cards
Pack cp = new StandardPack();
Deck deck = new Deck(cp);

System.out.println("Remaining cards: "
    + deck.remainingCards());
```

Furthermore, you can easily add more types of cards by extending the Card and Pack classes. The complete code is named DeckOfCards.

# Example 4: Parking lot

Amazon, Google, Adobe, Microsoft

Problem: Design the main classes of a parking lot.

What to ask: Is it a single-level or multi-level parking lot? Are all parking space spots the same? What type of vehicles should we park? Is it free parking? Do we use parking tickets?