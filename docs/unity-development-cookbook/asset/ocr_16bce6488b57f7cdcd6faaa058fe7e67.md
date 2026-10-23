When you multiply movement by Time.deltaTime, movement no longer represents a fixed distance, but rather a distance over time. When you provide this to the transform’s Translate method, you’ll find the object moving at a fixed speed, regardless of how quickly the renderer can produce new frames.

deltaTime isn’t the only property available to you. If you want to find out how long the project has been running, you can access the time variable. You can also slow down and speed up time by modifying the Time.timeScale property, which is quite useful for pausing the project.

# 2.4 Using the Components of a Game Object

# Problem

You want to make use of components attached to a game object.

# Solution

Use the GetComponent method to get a component that’s attached to the same game object as your script.

When you call GetComponent, you specify the type of component you’re looking for as a generic parameter. For example, to get a Renderer component on the game object, you call the method GetComponent<Renderer>.

![](bbox=[0.15, 0.545, 0.26, 0.66])

If there’s no component of the right type attached to the game object, GetComponent will return null. If you try to use this object by calling methods on it or accessing properties and fields, you’ll cause a NullPointerException to be thrown, which at the very least will immediately stop your script for the frame, and at worst can cause the entire project to crash. Always check first to see if the result is null.

Once you have a reference to another component, you can make use of it. For example, if you have a MeshRenderer, you can change the color of its material over time by accessing the component via GetComponent, and then accessing its properties:

```
public class ColorFading : MonoBehaviour
{
    void Update()
    {
        var meshRenderer = GetComponent<MeshRenderer>();
```