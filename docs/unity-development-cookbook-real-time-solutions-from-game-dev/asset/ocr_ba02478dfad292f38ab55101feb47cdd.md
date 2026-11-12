```
// The object we'll rotate to aim
[SerializeField] Transform weapon;

// The object we're trying to aim at
[SerializeField] Transform target;

// Aim at the target when it's within this range
[SerializeField] float range = 5f;

// The arc that we'll turn in while the target is out of range
[SerializeField] float arc = 45;

// The state machine that manages this object
StateMachine stateMachine;

// Use this for initialization
void Start() {
    // Create the state machine
    stateMachine = new StateMachine();

    // The first state we register will be the initial state
    var searching = stateMachine.CreateState("searching");

    // Log when we enter the state
    searching.onEnter = delegate {
        Debug.Log("Now searching for the target...");
    };

    // Each frame, animate the turret and also check to see if the
    // target is in range
    searching.onFrame = delegate {

        // Sweep from side to side
        var angle = Mathf.Sin(Time.time) * arc / 2f;
        weapon.eulerAngles = Vector3.up * angle;

        // Find the distance to our target
        float distanceToTarget =
            Vector3.Distance(transform.position, target.position);
```