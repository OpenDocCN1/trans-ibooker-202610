```
```java
if(!response.contains("Name")) {
    throw new AirportInfoException("Invalid airport code " + iata);
}

return response.split("/"[3]; //a bruteforce way to get the Name
}
}
}
```
```

The `getNameOfAirport` method takes an IATA code for an airport,
talks to a web service to get information about that airport,
and extracts the name of the airport from the response. Due
to the nature of the code—there may be failures accessing the
remote web service or the service may not recognize the
given code—failures are inevitable.

The `getNameOfAirport` method isn’t the right place to handle any
errors. If the call to the service fails, we merely propagate the
`IOException` from the URL to the caller of `getNameOfAirport`. If, on the
other hand, the service call succeeded but we didn’t get the
airport details we’re looking for, the `getNameOfAirport` method
throws a domain-specific checked exception `AirportInfoException`,
which is defined as shown next:

exceptionhandling/fpij/AirportInfoException.java

```
```java
package fpij;

public class AirportInfoException extends Exception {
    public AirportInfoException(String message) {
        super(message);
    }

    public AirportInfoException(Throwable cause) {
        super(cause);
    }

    public AirportInfoException(String message, Throwable cause) {
        super(message, cause);
    }
}
```
```