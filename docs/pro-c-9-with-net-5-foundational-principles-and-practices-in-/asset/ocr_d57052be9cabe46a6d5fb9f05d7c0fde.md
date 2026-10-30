# Building Finalizable and Disposable Types

At this point, you have seen two different approaches to constructing a class that cleans up internal unmanaged resources. On the one hand, you can use a finalizer. Using this technique, you have the peace of mind that comes with knowing the object cleans itself up when garbage collected (whenever that may be) without the need for user interaction. On the other hand, you can implement IDisposable to provide a way for the object user to clean up the object as soon as it is finished. However, if the caller forgets to call Dispose(), the unmanaged resources may be held in memory indefinitely.

As you might suspect, it is possible to blend both techniques into a single class definition. By doing so, you gain the best of both models. If the object user does remember to call Dispose(), you can inform the garbage collector to bypass the finalization process by calling GC.SuppressFinalize(). If the object user forgets to call Dispose(), the object will eventually be finalized and have a chance to free up the internal resources. The good news is that the object's internal unmanaged resources will be freed one way or another.

Here is the next iteration of MyResourceWrapper, which is now finalizable and disposable, defined in a C# Console Application project named FinalizableDisposableClass:

```
using System;

namespace FinalizableDisposableClass
{
    // A sophisticated resource wrapper.
    public class MyResourceWrapper : IDisposable
    {
        // The garbage collector will call this method if the object user forgets to call Dispose().
        ~MyResourceWrapper()
        {
            // Clean up any internal unmanaged resources.
            // Do **not** call Dispose() on any managed objects.
        }
        // The object user will call this method to clean up resources ASAP.
        public void Dispose()
        {
            // Clean up unmanaged resources here.
            // Call Dispose() on other contained disposable objects.
            // No need to finalize if user called Dispose(), so suppress finalization.
            GC.SuppressFinalize(this);
        }
    }
}
```

Notice that this Dispose() method has been updated to call GC.SuppressFinalize(), which informs the runtime that it is no longer necessary to call the destructor when this object is garbage collected, given that the unmanaged resources have already been freed via the Dispose() logic.

# A Formalized Disposal Pattern

The current implementation of MyResourceWrapper does work fairly well; however, you are left with a few minor drawbacks. First, the Finalize() and Dispose() methods each have to clean up the same unmanaged resources. This could result in duplicate code, which can easily become a nightmare to maintain. Ideally, you would define a private helper function that is called by either method.