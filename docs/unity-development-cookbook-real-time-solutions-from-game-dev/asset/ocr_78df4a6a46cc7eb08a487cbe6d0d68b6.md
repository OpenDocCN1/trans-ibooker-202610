```
// Whether this wheel can apply brakes
public bool hasBrakes;
}
```

```
// Controls the power, braking, and steering applied to wheels.
public class Vehicle : MonoBehaviour {
```

```
// The list of wheels on this vehicle
[SerializeField] Wheel[] wheels = {};
```

```
// The settings used for controlling the wheels:
```

```
// Maximum motor torque
[SerializeField] float motorTorque = 1000;
```

```
// Maximum brake torque
[SerializeField] float brakeTorque = 2000;
```

```
// Maximum steering angle
[SerializeField] float steeringAngle = 45;
```

```
private void Update() {
```

```
// If the vertical axis is positive, apply motor torque and no
// brake torque. If it's negative, apply brake torque and no
// motor torque.
```

```
var vertical = Input.GetAxis("Vertical");
```

```
float motorTorqueToApply;
float brakeTorqueToApply;
```

```
if (vertical >= 0) {
    motorTorqueToApply = vertical * motorTorque;
    brakeTorqueToApply = 0;
}
```