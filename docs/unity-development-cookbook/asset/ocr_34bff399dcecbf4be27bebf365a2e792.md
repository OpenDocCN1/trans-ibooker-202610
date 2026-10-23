Rather than being destroyed completely via the Destroy method, objects are returned to the pool. To simplify things, this recipe adds an extension method on the Game Object class, which makes it as easy to return an object to the pool as it is to destroy it.

# 2.9 Storing Data in Assets Using ScriptableObject

# Problem

You want to store data inside asset files that your scripts can access.

# Solution

Create a subclass of the ScriptableObject class, and add the properties you want in there.

For example, let’s create an asset that stores a color, and a script that uses that asset to set its renderer’s color when the game starts. We’ll start by creating the color-storing asset:

+   1. Create a new C# script called ObjectColour.cs, and add the following code to it:

```
// Create an entry in the Asset -> Create menu that makes it easy to
// create a new asset of this type
[CreateAssetMenu]

// Don't forget to change the parent class from 'MonoBehaviour' to
// 'ScriptableObject'!
public class ObjectColour : ScriptableObject
{
    public Color color;
}
```

The [CreateAssetMenu] attribute tells Unity to add a new entry in the Assets → Create menu, which lets you create a new asset file that stores an instance of the ScriptableObject subclass.

Because the object is stored on disk as an asset, it can be dragged and dropped into the Inspector of other objects across multiple scenes, just as you can with other assets, like models and textures.

![](bbox=[0.18, 0.79, 0.27, 0.87])

You can use any types of variables in a ScriptableObject that you can use in a MonoBehaviour subclass.