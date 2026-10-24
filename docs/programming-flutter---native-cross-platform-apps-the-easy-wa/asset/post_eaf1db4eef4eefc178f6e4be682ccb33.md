
those classes. The StarredPage’s fetchComic is called only inside _retrieveSavedComics and the HomeScreen’s is set as the future of a FutureBuilder inside the build method.

## Testing fetchComic

This is the code to test the SelectionPage fetchComic’s ability to fetch comic number 2:

testing/xkcd_app/test/unit_test.dart

```
test("SelectionPage fetchComic latest", () async {
  var latestComicFile = MyFile();
  var latestComicExists = false;
  String latestComicString;
  var latestImageFile = MyFile();
  Uint8List latestImageData;
  var latestImageExists = false;
  var mockHttp = MockHTTPClient();

  when(mockHttp.read('https://xkcd.com/2/info.0.json')).thenAnswer(
    (_) => Future.value(comics[1])
  );

  when(mockHttp.readBytes("https://example.com/2.png")).thenAnswer(
    (_) => Future.value(Uint8List.fromList([2]))
  );

  when(latestComicFile.createSync()).thenAnswer(
    (_) {
      latestComicExists = true;
    }
  );

  when(latestComicFile.create()).thenAnswer(
    (_) {
      latestComicExists = true;
      return Future.value(latestComicFile);
    }
  );

  when(latestComicFile.writeAsStringSync(comics[1])).thenAnswer(
    (_) {
      latestComicExists = true;
      latestComicString = comics[1];
    }
  );

  when(latestComicFile.writeAsString(comics[1])).thenAnswer(
    (_) {
      latestComicExists = true;
      latestComicString = comics[1];
      return Future.value(latestComicFile);
    }
  );

  when(latestComicFile.existsSync()).thenReturn(
    latestComicExists
  );

when(latestComicFile.exists()).thenAnswer(
    (_) =>
        Future.value(latestComicExists)
);

when(latestComicFile.readAsStringSync()).thenAnswer(
    (_) {
        /*
         * if we try to read from a file that doesn't
         * exist it's because we've made a mistake
         */
        assert(latestComicExists, true);
        return latestComicString;
    }
);

when(latestImageFile.readAsBytes()).thenAnswer(
    (_) {
        assert(latestImageExists, true);
        return Future.value(latestImageData);
    }
);

when(
    latestImageFile.writeAsBytesSync(
        Uint8List.fromList([1])
    )
).thenAnswer(
    (_) {
        latestImageExists = true;
        latestImageData = Uint8List.fromList([1]);
    }
);

when(latestComicFile.writeAsString(comics[1])).thenAnswer(
    (_) {
        latestImageExists = true;
        latestImageData = Uint8List.fromList([1]);
        return Future.value(latestImageFile);
    }
);

var selPage = SelectionPage();

expect(
    await selPage.fetchComic(
        "2",
        httpClient: mockHttp,
        comicFile: File("latestComicFole"),
        imageFile: File("latestComicFile"),
        imagePath: "https://example.com/2.png"
    ),
    json.decode(comics[1])
);
```

It’s nothing new: we use Mockito to define expected behavior for all functions that may be called by the fetchComic method to fetch the comic: reading and writing from image and comic files and performing HTTP requests.

Defining new tests for other `fetchComic` methods only requires changes for the `HomeScreen`: it needs an integer for the comic number, which is a number that starts from 0 and can be, at most, 1 less than the latest comic number.

## Grouping Together Tests

Multiple tests can be grouped together using the `group` method, which works exactly like `test`.

```
group("group name", () {
  test("first test in group", () {
    // First test in group
  });
  test("second test in group", () {
    // Second test in group
  });
  // any other tests you want to add
});
```

For example, we can group all tests that involve `fetchComic` methods, or maybe we can group together all tests that fetch a specific comic if you decide to structure tests differently. Inside groups we can, for example, define variables and mock objects that are needed for more than one test, so that they are accessible by all tests in a group but not outside of the group, but you need to keep in mind that the group’s callback closure can’t be async.

> **More Unit Tests: Comic Starring**
> 
> Testing comic starring in the `StarredPage` doesn’t introduce particularly challenging situations, except for the fact that the `retrieveSavedComics` method calls `fetchComic`, which means that the mock objects need to be passed first to `retrieveSavedComics` which will, in turn, pass it to `fetchComic`.
> 
> The overall structure of the test should be the following: the `addToStarred` method (in the `ComicPage`) should be called on the same file that will then be used by `retrieveStarredComics` to get the starred comics list. The main difference between this test and the `fetchComic` test is that this one needs two mock text files: one of them needs to contain the comic file, whereas the other one will store the list of saved comics numbers, so it needs to predict the data it will be asked to store, just like we did earlier for the comic and latest number files.
> 
> Writing these tests as someone who is trying to understand Flutter will also help you understand the inner workings of our app in more detail and test your own understanding of it. If you can’t write a test for it, you probably don’t understand how it’s working and that means you need to try to first understand how the app works.

## A FEW WORDS ON WIDGET TESTING MORE COMPLICATED APPS

We have already covered widget testing in the section about testing the calculator, so we won't have examples of widget tests for the XKCD apps, which would be redundant.

Widget testing the XKCD app involves testing whether or not the UI actually works, so we don't actually need to be necessarily design mock objects that perfectly emulate the behavior of the network or file system: we just need to know the UI we need exists and is correctly interacting with our app's backbone.

Widget tests rely on mock objects to work, so reducing the scope of these tests to just testing whether the widgets call the functions they are supposed to call is all that should be done in widget tests: doing anything more than that will only make the tests more complicated and make the integration tests partially redundant.

Verifying Whether a Method Has Been Called: Mockito's verify

Mockito includes a function to verify whether or not a method has been called, making it possible to reduce the scope of tests to only verifying whether methods get called instead of necessarily checking whether a given method returns a given value. For example, if you want the test to fail if `myObj.runMethod()` hasn't been called, you'd add:

```
verify(myObj.runMethod());
```

If you replaced `verify` with `verifyNever` you'd have the opposite effect.

You can also specify the amount of times a method should get called:

```
verify(myObj.runMethod()).called(5);
```

requires the method to have been called 5 times and:

```
verify(myObj.runMethod()).called(greaterThan(4));
```

requires it to have been called *at least* 5 times.

unfortunately, not supported by Flutter’s integration testing infrastructure (at the time of writing), which means we have to create a `TextEditingController` and a separate `FlatButton` to submit the `TextField`’s contents.

## Defining a TextEditingController

A `TextEditingController` is a class that stores the data inserted in a `TextField` or a similar widget and makes it possible to access that data from outside the `onSubmitted` callback we give to the `TextField` itself, allowing us to create a button that opens the comic the user asked for, and giving us something to tap to do that when performing integration tests.

Let’s declare a `TextEditingController` as a private member variable of the `SelectionPage`:

```
final TextEditingController _controller = TextEditingController();
```

## Using the TextEditingController

Let’s pass it as the `controller` argument to the `TextField`:

```
TextField(
  controller: _controller,
  decoration: InputDecoration(
    labelText: "Insert comic #",
  ),
)
```

## Adding the FlatButton

We now need to turn the `Center` widget in the `body` of the `Scaffold` into a `Column`, which will also require changing the `child` argument to `children`, which needs to be a `List` literal (we need to enclose the `TextField` definition in square brackets, in which we’ll also add the `FlatButton`).

The `FlatButton` we’re going to add needs a `key` to be used in integration testing, and it needs to perform the same actions performed by the `TextField`’s `onSubmit`, but by taking
