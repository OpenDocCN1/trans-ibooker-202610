> **NOTE**
> 
> This code won't compile yet, because the PooledObject class doesn't exist yet. We're about to add it.

In order for objects to be able to return to the pool that created them, they need to have a reference to them. To support this, we'll create a very simple script that has no logic, and stores a reference to the object pool.

> **TIP**
> 
> Because this script doesn't really influence gameplay, there's no reason to include it in the Inspector. This is what the hideFlags property controls; by setting it to HideInInspector in the GetObject method, we're telling Unity to not display it. This is convenient for this use case, but be careful about using this feature, because it's easy to forget that a component exists if you can't see it. You might accidentally write a component that does some behavior that you can't understand because you don't realize that the component exists.

+   1. Create a new script called PooledObject.cs, and add the following code to it:

```
```csharp
// A simple script that just exists to store a reference to an
// ObjectPool. Exists to be used by the ReturnToPool extension method.
public class PooledObject : MonoBehaviour
{
    public ObjectPool owner;
}
```
```

To simplify the process of returning a GameObject to an object pool, we'll add a method to GameObject that makes it very convenient. To do this, we add an extension method called ReturnToPool; once this code exists, you can call ReturnToPool on any GameObject, and it will return to the pool that created it.