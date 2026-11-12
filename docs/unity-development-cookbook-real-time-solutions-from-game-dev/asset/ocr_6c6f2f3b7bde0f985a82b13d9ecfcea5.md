**NOTE**

In this recipe, we're using more interfaces than you'd probably use in a real situation (for example, we're handling pointer up and down *and* clicks), but it serves as a good demonstration. These aren't the only events you can register to receive method calls for; check the Unity documentation for more.

8. Next, we'll add code that makes it simpler to modify the color of the object. Add the following variable and method to the ObjectMouseInteraction class:

```
Material material;

void Start() {
    material = GetComponent<Renderer>().material;
}
```

9. Finally, we'll add the methods that will be called when the cursor moves over or clicks the object. Add the following methods to the ObjectMouseInteraction class:

```
public void OnPointerClick(PointerEventData eventData)
{
    Debug.LogFormat("{0} clicked!", gameObject.name);
}

public void OnPointerDown(PointerEventData eventData)
{
    Debug.LogFormat("{0} pointer down!", gameObject.name);
    material.color = Color.green;
}
```