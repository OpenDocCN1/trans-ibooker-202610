The following script demonstrates how to use player input to apply forces to an object:

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
    // FixedUpdate, because it's called a fixed number of times per second,
    // with a fixed simulation timestep, which means more stable simulation
    void FixedUpdate() {
        // Get user input, and scale it by the amount of force we want to
        // apply
        var vertical = Input.GetAxis("Vertical") * verticalForce;
        var horizontal = Input.GetAxis("Horizontal") * sidewaysForce;

        // Generate a force vector from these inputs, scaled by time
        var force =
            new Vector2(horizontal, vertical) * Time.deltaTime;

        // Add the force to the sprite
        body.AddForce(force);
    }
}
```

![](bbox=[0.15, 0.62, 0.25, 0.72])

It's not especially practical for such a small example, but input would typically be collected in Update, rather than FixedUpdate as we did here. FixedUpdate can run more than once per render frame, which can mean you end up with extra input!

# Discussion

Unlike Rigidbody components, which operate in three dimensions, Rigidbody2D components operate only in two dimensions. This means that all the forces you apply are Vector2D, rather than Vector3D. Additionally, because there's only one degree of rotational freedom, you can apply rotation only on one axis.