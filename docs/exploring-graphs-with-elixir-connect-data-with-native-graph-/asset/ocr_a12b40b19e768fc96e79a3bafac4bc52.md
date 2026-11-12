can flatMap, not map, since we have a _one-to-many_ relationship. Since this is in the collect phase, we'll use flatMapping instead of flatMap to store the email addresses into each group. But we want only email addresses that end with ".com", so we need to use the filtering method. While the filter operation at the level of a stream is called filter, when done during collect, it's called filtering. Let's take a look at the code:

```
var lastNamesAndEmailAddressesFiltered = SAMPLE_DATA.stream() .collect( groupingBy(Person::lastName, flatMapping(person -> person.emailAddresses().stream(), filtering(address -> address.endsWith(".com"), toList())))); System.out.println(lastNamesAndEmailAddressesFiltered);
```

Much like flatMap, the flatMapping method expects the lambda expression passed to it to return a Stream. In this example, we pass the result of that to the filtering operation, which lets only those email addresses ending with ".com" go through. We finally pack the resulting email addresses into a list. Let's take a glance at the output to confirm the code worked as expected:

```
{
Xie=[nancy@example.com, xie@example.com, nx@example.com],
Baker=[mike@example.com, baker@example.com],
Smith=[jill@example.com],
Walker=[sara@example.com],
Shah=[dev@example.com, shah@example.com],
Doe=[],
Lee=[lee@example.com]
}
```