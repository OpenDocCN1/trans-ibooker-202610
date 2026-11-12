```
[SerializeField] float speed = 1f;

// A cached reference to the renderer (The 'new' keyword makes the
// compiler not warn us about the fact that we're overriding an
// existing property that we inherit from MonoBehaviour; this property
// is deprecated, so we aren't using it and it's okay to override it)
new Renderer renderer;

private void Start()
{
    renderer = GetComponent<Renderer>();
}

private void Update()
{
    // Convert time to a number that smoothly moves between -1 and 1
    float t = Mathf.Sin(Time.time * speed);

    // Convert this to one that moves from 0 to 2
    t += 1;

    // Divide it by 2 to make it move from 0 to 1;
    t /= 2;

    // Interpolate between the two colors
    var newColor = Color.Lerp(fromColor, toColor, t);

    // Apply the new color
    renderer.material.color = newColor;
}
}
```

# Discussion

There are two ways you can access the material used by a renderer: the material property and the sharedMaterial property.

The first time you access the renderer's material property, Unity creates a copy of the material that the renderer is currently using, tells the renderer to use this new copy, and returns it. This is because if multiple renderers all use the same material file, you likely don't want to change all of them when you change one of them.