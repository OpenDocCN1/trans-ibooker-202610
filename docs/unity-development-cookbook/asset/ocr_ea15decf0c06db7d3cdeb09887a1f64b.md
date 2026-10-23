```
// If the new state has an on enter method, call it
if (newState.onEnter != null)
{
    newState.onEnter();
}

// Transitions to a named state.
public void TransitionTo(string name) {
    if (states.ContainsKey(name) == false) {
        Debug.LogErrorFormat("State machine doesn't contain a state" + " named {0}!", name);
        return;
    }
    // Find the state in the dictionary
    var newState = states[name];
    // Transition to it
    TransitionTo(newState);
}
```

This StateMachine class can be used like this:

```
// Demonstrates a state machine. This object has two states: 'searching'
// and 'aiming'. When the target is in range, it transitions from
// 'searching' to 'aiming'; when the target leaves range, it
// transitions back.
public class Turret : MonoBehaviour {
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
```