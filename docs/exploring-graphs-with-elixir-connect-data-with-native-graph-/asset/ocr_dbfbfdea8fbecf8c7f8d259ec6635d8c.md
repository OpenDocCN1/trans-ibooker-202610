The error is from the lambda expression we're passing to
the map method. The parameter of Stream's map method takes
Function<T, R> as its parameter. The abstract method of that
functional interface, R apply(T) doesn't have any throws clause.
Thus, the lambda expression or method reference that will
be used as an argument to map isn't allowed to throw any
checked exceptions.

That seems like a serious limitation at first glance. Some
developers have complained about it. Some have even
taken the extreme measure of forking the JDK code and
adding the throws clause. But the folks behind the Java
language have done what is right—it's not an oversight.
Let's see why.