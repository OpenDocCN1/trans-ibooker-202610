> **NOTE**
> 
> After creating the Event System, if you've configured your project to use the Unity Input System (see Recipe 3.2), it will warn you that you will need to update the component to one that works with the Input System. To do this, click Replace with InputSystemUIModule.

To access the classes and interfaces we need for working with the pointer, we need to access the UnityEngine.EventSystems namespace. Once that's done, we make our class conform to certain interfaces that mark the MonoBehaviour as something that needs to know about input events.

+   7. Delete the ObjectMouseInteraction class from the code, and replace it with the following code:

```
public class ObjectMouseInteraction :
    MonoBehaviour,
    IPointerEnterHandler, // Handles mouse cursor entering object
    IPointerExitHandler, // Handles mouse cursor exiting object
    IPointerUpHandler, // Handles mouse button lifting up on this
    // object
    IPointerDownHandler, // Handles mouse button being pressed on this
    // object
    IPointerClickHandler // Handles when mouse is pressed and
    // released on this object
{
}
```

By implementing these interfaces, Unity will know to contact this script when input events happen to the attached object.