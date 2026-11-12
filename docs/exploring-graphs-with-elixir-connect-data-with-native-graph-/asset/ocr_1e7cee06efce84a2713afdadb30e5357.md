```
var iataCodes = List.of("AUS", "DFW", "HOU", "IHA", "SAT");
getNamesOfAirports(iataCodes).stream()
.map(name -> switch(name) {
case Success(String result) -> result;
case Failure(Throwable throwable) -> "Error: " + throwable.getMessage();
})
.forEach(System.out::println);
}
```

We've pushed the error handling all the way to the edge where we can align the code to the business requirements. By preserving all the information in a strongly typed form, we can gracefully evolve the program as the requirements change. The main method gets a list of Try instances from the getNamesOfAirports method and iterates over it using the stream internal iterator. In the map call, we extract the result from the Try if it is a success; otherwise, we extract the error details.

We're using the amazing feature of the switch expression of Java. If you're using an older version of Java, you may use the traditional if-else instead—the code will be a tad verbose but will get the job done.

Run the code and take a look at the output:

```
AUSTIN-BERGSTROM INTL
DALLAS-FORT WORTH INTL
WILLIAM P HOBBY
Error: Invalid airport code IHA
SAN ANTONIO INTL
```

It worked...and the output is exactly the same as the output of the imperative style version. Phew!

Before we end this chapter, let's bring together everything we've covered on handling errors in functional style code.