```
}

public abstract class Card {

    private final Enum suit;
    private final int value;

    private boolean available = Boolean.TRUE;

    public Card(Enum suit, int value) {
        this.suit = suit;
        this.value = value;
    }

    // code omitted for brevity
}

public class StandardCard extends Card {

    private static final int MIN_VALUE = 1;
    private static final int MAX_VALUE = 13;

    public StandardCard(StandardSuit suit, int value) {
        super(suit, value);
    }

    // code omitted for brevity
}
```

+   - Standard pack of cards implementation gives the following code:

```
public abstract class Pack<T extends Card> {

    private List<T> cards;

    protected abstract List<T> build();
```