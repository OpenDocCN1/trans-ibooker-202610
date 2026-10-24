# # Testing the XKCD App: Using Mock Objects

Testing the XKCD app presents a challenge: the app relies on external variables: an Internet connection needs to be present and the app needs to be able to connect to the XKCD servers (which need to be up) to work. The issue with that is that we may need to only test a small part of our app (especially during unit testing) without needing to interface with anything else, and we need to mock files since we can’t access them anyway. To do this, developers usually resort to building *mock objects*, which are objects that simulate the features provided by the external interface while not requiring that interface.

Building mock objects isn’t language or framework specific, but Google has developed a Flutter package that allows developers to simplify the creation of mock objects: the Mockito library.

# ## THE MOCKITO LIBRARY

The Google-developed *mockito* package on Pub[^44] allows you to write classes that reproduce a behavior you choose by using the `when` function.

# ### Writing a Mock Object

As an example, let’s write a replacement for the `http` constant we use in the XKCD app to make requests so that we can test the `getLatestNumber` and `fetchComic` methods. It is an object of the class `Client`, so we write an empty class that extends (*extends*) Mock and implements (*Writing a Class That implements an Interface*) Client’s members:

```
```dart
import 'package:http/http.dart' as http;

class MockHTTPClient extends Mock implements http.Client {}
```
```

Let’s define two comics we can return:

```
```dart
// testing/xkcd_app/test/unit_test.dart
```
```