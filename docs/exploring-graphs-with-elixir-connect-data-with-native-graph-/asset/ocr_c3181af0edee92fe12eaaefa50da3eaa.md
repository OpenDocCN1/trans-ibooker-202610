fetching prices for any ticker symbol, you'll have to get your own token. Instead of going through that effort, we'll create a function in this chapter to return some fake prices for tickers. Here's the code for that:

```
applying/fpij/FinanceData.java

public class FinanceData {
    public static BigDecimal getPrice(final String ticker) {
        Map<String, String> fakePrices = new HashMap<>() {
            {
                put("AMD", "81"); put("HPQ", "33"); put("IBM", "135");
                put("TXN", "150"); put("VMW", "116"); put("XRX", "15");
                put("AAPL", "131"); put("ADBE", "360"); put("AMZN", "106");
                put("CRAY", "130"); put("CSCO", "43"); put("SNE", "72");
                put("GOOG", "2157"); put("INTC", "36"); put("INTU", "369");
                put("MSFT", "247"); put("ORCL", "67"); put("TIBX", "24");
                put("VRSN", "157"); put("RIVN", "26");
            }
        };

        try { Thread.sleep(200); } catch(Exception ex) {} //simulate a call delay

        return new BigDecimal(fakePrices.get(ticker));
    }
}
```

The getPrice method returns some fake price for a given ticker symbol. Since we're looking for only stocks valued over $100, we can use Stream's filter method to trim down the list. Once we get the short list, we can sort it easily using Stream's sorted method. Finally, we can concatenate the symbols to print. These are all operations we've seen before, coming together here to help with this task. Let's look at the code.

```
applying/fpij/Stocks100.java

final BigDecimal HUNDRED = new BigDecimal("100");

System.out.println("Stocks priced over $100 are:");
```