The current implementation of MediaPlayer assumes that the object user will want to obtain a list of songs via the GetAllTracks() method. Well, what if the object user does not need to obtain this list? In the current implementation, the AllTracks member variable will still be allocated, thereby creating 10,000 Song objects in memory, as follows:

```
using System;
using LazyObjectInstantiation;

Console.WriteLine("***** Fun with Lazy Instantiation *****
");

// This caller does not care about getting all songs,
// but indirectly created 10,000 objects!
MediaPlayer myPlayer = new MediaPlayer();
myPlayer.Play();

Console.ReadLine();
```

Clearly, you would rather not create 10,000 objects that nobody will use, as that will add a good deal of stress to the .NET Core garbage collector. While you could manually add some code to ensure the _allSongs object is created only if used (perhaps using the factory method design pattern), there is an easier way.

The base class libraries provide a useful generic class named Lazy<>, defined in the System namespace of mscorlib.dll. This class allows you to define data that will not be created unless your code base actually uses it. As this is a generic class, you must specify the type of item to be created on first use, which can be any type with the .NET Core base class libraries or a custom type you have authored yourself. To enable lazy instantiation of the AllTracks member variable, you can simply update the MediaPlayer code to this:

```
// The MediaPlayer has-an Lazy<AllTracks> object.
class MediaPlayer
{
...
    private Lazy<AllTracks> _allSongs = new Lazy<AllTracks>();
    public AllTracks GetAllTracks()
    {
        // Return all of the songs.
        return _allSongs.Value;
    }
}
```

Beyond the fact that you are now representing the AllTracks member variable as a Lazy<> type, notice that the implementation of the previous GetAllTracks() method has also been updated. Specifically, you must use the read-only Value property of the Lazy<> class to obtain the actual stored data (in this case, the AllTracks object that is maintaining the 10,000 Song objects).

With this simple update, notice how the following updated code will indirectly allocate the Song objects only if GetAllTracks() is indeed called:

```
Console.WriteLine("***** Fun with Lazy Instantiation *****
");

// No allocation of AllTracks object here!
MediaPlayer myPlayer = new MediaPlayer();
myPlayer.Play();
```