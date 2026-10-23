
2. Next, create a raycaster component that the Event System will use to determine what objects are under the cursor. Raycasters trace lines from the camera position through the cursor position and into the scene to detect what a ray hits. If the ray hits a collider, then the Event System dispatches events to the target object.

3. Select the main Camera object.

4. Open the Component menu, and choose Event → Physics Raycaster. (You can also add it by clicking the Add Component button at the bottom of the Inspector, and choosing Event → Physics Raycaster.)

![](img/e54c00769db669e3746fa1e50090241f_80_0.png)

In this recipe, we use 3D objects by utilizing the 3D physics system. For a 2D game, the same techniques apply, but you’ll add a Physics 2D Raycaster to your camera instead of a Physics Raycaster. The Physics Raycaster detects only 3D colliders, and the Physics 2D Raycaster detects only 2D colliders. If you’re using both collider types in your scene, both of them can be on your camera at the same time.

5. Next, we’ll write a script that detects when a cursor moves over the object that the script is attached to and changes the color of its renderer. Create a new C# script called ObjectMouseInteraction.cs.

6. Add the following using directive to the top of the class:

```
using UnityEngine.EventSystems;
```

![](img/e54c00769db669e3746fa1e50090241f_80_1.png)

After creating the Event System, if you’ve configured your project to use the Unity Input System (see Recipe 3.2), it will warn you that you will need to update the component to one that works with the Input System. To do this, click Replace with InputSystemUIModule.

To access the classes and interfaces we need for working with the pointer, we need to access the UnityEngine.EventSystems namespace. Once that’s done, we make our class conform to certain interfaces that mark the MonoBehaviour as something that needs to know about input events.

7. Delete the ObjectMouseInteraction class from the code, and replace it with the following code:

```
public class ObjectMouseInteraction :
    MonoBehaviour,
    IPointerEnterHandler,  // Handles mouse cursor entering object
    IPointerExitHandler,   // Handles mouse cursor exiting object
    IPointerUpHandler,     // Handles mouse button lifting up on this
                           // object
    IPointerDownHandler,   // Handles mouse button being pressed on this
                           // object
    IPointerClickHandler   // Handles when mouse is pressed and
                           // released on this object
{
}
```

By implementing these interfaces, Unity will know to contact this script when input events happen to the attached object.

![](img/e54c00769db669e3746fa1e50090241f_81_0.png)

In this recipe, we’re using more interfaces than you’d probably use in a real situation (for example, we’re handling pointer up and down and clicks), but it serves as a good demonstration. These aren’t the only events you can register to receive method calls for; check the Unity documentation for more.

8. Next, we’ll add code that makes it simpler to modify the color of the object. Add the following variable and method to the ObjectMouseInteraction class:

```
Material material;

void Start() {
    material = GetComponent<Renderer>().material;
}
```

9. Finally, we’ll add the methods that will be called when the cursor moves over or clicks the object. Add the following methods to the ObjectMouseInteraction class:

```
public void OnPointerClick(PointerEventData eventData)
{
    Debug.LogFormat("{0} clicked!", gameObject.name);
}
```

```
public void OnPointerDown(PointerEventData eventData)
{
    Debug.LogFormat("{0} pointer down!", gameObject.name);
    
    material.color = Color.green;
}

public void OnPointerEnter(PointerEventData eventData)
{
    Debug.LogFormat("{0} pointer enter!", gameObject.name);
    
    material.color = Color.yellow;
}

public void OnPointerExit(PointerEventData eventData)
{
    Debug.LogFormat("{0} pointer exit!", gameObject.name);
    
    material.color = Color.white;
}

public void OnPointerUp(PointerEventData eventData)
{
    Debug.LogFormat("{0} pointer up!", gameObject.name);
    
    material.color = Color.yellow;
}
```

We’re now done with the code, and can put it to use. We’ll create a cube that has this component attached; it will change its color when the mouse interacts with it:

- 1. Open the GameObject menu, and choose 3D Object → Cube.
- 2. Place the new cube somewhere in front of the camera.
- 3. Drag the ObjectMouseInteraction.cs script onto the cube.

Play the game. As the mouse moves over it or clicks it, the cube changes color.

## Discussion

Remember that the camera’s Physics Raycaster component can only “see” colliders. Objects without colliders won’t receive these events. You can also debug the Event System by looking at the Event System object’s Inspector. During play, it displays information about where the cursor is and what object currently receives a raycast (Figure 3-1).

Figure 3-1. The debugging information in the Inspector of an Event System object

## CHAPTER 4

## Mathematics

Mathematics is an unavoidable part of using a spatial development tool like Unity. Like it or loathe it, you need to use it, acknowledge it, and even downright embrace it to get anything done. Fortunately, you can gloss over or pretend a lot of it isn’t there, but it’s always there, supporting you, and lurking beneath.

This chapter explores some of the most common math-related problems you’ll encounter when developing using Unity.

![](img/e54c00769db669e3746fa1e50090241f_85_0.png)

This isn’t all the math you’ll need for game, or Unity, development; far from it! But it’s enough math to help out, and get you started. We’ve done our best to structure it around the problems you might want to solve using the math, rather than the math itself.

Have you ever wondered what a vector is? Or how to use a matrix? What about a quaternion? We’ll cover them here, while also touching on common math problems in Unity development, like calculating the angles and distances to something else in your Unity-based project.

![](img/e54c00769db669e3746fa1e50090241f_85_1.png)

Nothing in this chapter is unique or specific to development with Unity. We do cover things from a Unity-centric perspective when we talk about code, but everything here is just plain old math for spatial or game development.

Read on for just enough math for Unity development.

![](img/e54c00769db669e3746fa1e50090241f_86_0.png)

We can’t more highly recommend Paco Nathan’s *Just Enough Math* video series, also from O’Reilly. Check it out if you want to learn even more math.

### 4.1 Storing Coordinates of Varying Dimensions Using Vectors

#### Problem

Unity development involves a lot of coordinates of varying dimensions:

- One-dimensional coordinates, such as 1 or −9
- Two-dimensional coordinates, such as 8, 3 or −1, 4
- Three-dimensional coordinates, such as 3, 1, −3 or −7, 6, 1

Realistically, when you’re building something in Unity, as long as you’re keeping track of what you need to keep track of, it often doesn’t matter how you do it. However, one common, useful way of keeping track of coordinates is vectors.

Vectors are a set of coordinates of varying dimensions. Thus, our problem here is: how do I use vectors and Unity, and what can I use them for?

#### Solution

To provide a solution here, we’re going to have to unpack the problem a little. We admit, we wrote a deliberately broad problem statement, but that’s so we can show you everything you need to know about vectors without having to have one tiny recipe for each manipulation or action you might want to make with a vector.

![](img/e54c00769db669e3746fa1e50090241f_86_1.png)

You’ll often see quite specific meanings applied to vectors: in video games it’s usually coordinates in geometry, but there’s absolutely no reason you can’t store anything you want in them. They’re just a data structure.

First up, in Unity, you can define a Vector2: one with two dimensions, usually an x and a y. A Vector2 is typically used to represent a point in 2D space in Unity.

You can define a Vector2 with two dimensions:

```
Vector2 direction = new Vector2(0.0f, 2.0f);
```
