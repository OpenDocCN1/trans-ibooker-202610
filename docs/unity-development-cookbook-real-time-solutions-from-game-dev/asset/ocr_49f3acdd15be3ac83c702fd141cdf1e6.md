renderer needs to display its sprite, the subsection of the atlas that contains
the necessary data is used.

Texture atlasing can lead to better performance. If multiple sprite renderers
are using sprites that are stored in the same atlas, they can be drawn at the
same time. Additionally, the packing process trims the edges from a sprite,
which reduces the amount of memory needed to store the final image.

You don’t need to do anything to use an atlas, besides creating the atlas
itself. Sprite renderers will automatically use an atlas texture if the sprite
they need to draw is stored inside one.

# 5.8 Applying Forces to 2D Objects

# Problem

You want to apply a physical force to a 2D object.

# Solution

You can apply forces to a 2D object through a script, using the
Rigidbody2D class’s AddForce method.

The following script demonstrates how to use player input to apply forces
to an object:

```
public class AlienForce : MonoBehaviour {

    // The amount of force to apply.
    [SerializeField] float verticalForce = 1000;
    [SerializeField] float sidewaysForce = 1000;

    // A cached reference to the Rigidbody2D on this object.
    Rigidbody2D body;

    // On game start, get the reference to the rigid body and cache it.
    void Start() {
        body = GetComponent<Rigidbody2D>();
    }

    // You will get smoother movement if you apply physical forces in
```