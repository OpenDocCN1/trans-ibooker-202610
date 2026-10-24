![](bbox=[0.14, 0.09, 0.28, 0.13])

# WHAT IF THAT COMIC DOESN'T EXIST?

The user could input any number that goes through their head in the SelectionPage. While I don’t actively endorse abusing innocent app features this way, the user could choose to input a number that is bigger than the latest comic’s number, and we can’t just let the app crash or let the CircularProgressIndicator loop forever because the user isn’t supposed to do that.

We need to handle that situation, and for that we’ll use the AsyncSnapshot’s hasError property: if it is true, it means an error occurred during the processing of the future. If it is true, we’ll show the user an ErrorPage, which we’ll build later.

Meanwhile, here’s the new builder for our FutureBuilder:

networking/xkcd_app/lib/main.dart

```
builder: (context, snapshot) {
  if(snapshot.hasError) return ErrorPage();
  if(snapshot.hasData) return ComicPage(snapshot.data);
  return CircularProgressIndicator();
}
```

I said we were going to build a ComicPage at some point. The moment has come to do that, so let’s do it! The ComicPage needs to be just a simple StatelessWidget and the usual Scaffold and AppBar. The body will be a Column containing an icon and a Text informing the user they have done something wrong:

networking/xkcd_app/lib/main.dart

```
class ErrorPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Error Page"),
      );
      body: Column(
        children: [
          Icon(Icons.not_interested),
          Text(
            "The comics you have selected doesn’t exist"+

```