![](bbox=[0.166, 0.096, 0.783, 0.285])

![](bbox=[0.155, 0.305, 0.835, 0.345])

Let's work with an example to get a better feel for this. We'll start with a list of ticker symbols and, from it, create a sorted list, with each item's corresponding stock valued over $100. In the habitual approach, we'd walk through the list using an external iterator and update a mutable collection. Instead, we'll transform objects. We'll filter the tickers list into a list of tickers priced over $100, then sort the list, and finally report.

We need a sample list of ticker symbols, so let's start with that.

# applying/fpij/Tickers.java

```
public class Tickers {
  public static final List<String> symbols = Arrays.asList(
    "AMD", "HPQ", "IBM", "TXN", "VMW", "XRX", "AAPL", "ADBE",
    "AMZN", "CRAY", "CSCO", "SNE", "GOOG", "INTC", "INTU",
    "MSFT", "ORCL", "TIBX", "VRSN", "RIVN");
}
```

We have some twenty symbols in this sample list. We need to determine the price for each stock. We saw the code to fetch the latest price from a web service in Integrating with a Web Service. That service works only for a few tickers when the hardcoded api_token is used. If you'd like to use that service for