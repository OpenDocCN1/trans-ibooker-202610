
## Step C: Compute Collision Tangent Direction

```javascript
// Step C: Compute collision tangent direction
let tangent = [0, 0];
vec2.scale(tangent, n, rVelocityInNormal);
vec2.subtract(tangent, tangent, relativeVelocity);
vec2.normalize(tangent, tangent);
// Relative velocity in tangent direction
let rVelocityInTangent = vec2.dot(relativeVelocity, tangent);
```

## Step D: Determine Effective Coefficients

**Step D**: Determine the effective coefficients by using the average of the colliding objects. As in the previous project, for consistency, friction coefficient is one minus the values form the `RigidShape` objects.

```javascript
// Step D: Determine the effective coefficients
let newRestituion = (a.getRestitution() + b.getRestitution()) * 0.5;
let newFriction = 1 - ((a.getFriction() + b.getFriction()) * 0.5);
```

## Step E: Impulse in Normal and Tangent Directions

**Step E**: Impulse in the normal and tangent directions, these are computed by following Equations (26) and (27) exactly.

```javascript
// Step E: Impulse in the normal and tangent directions
// R cross N
let rBPcrossN = rBP[0] * n[1] - rBP[1] * n[0]; // rBP cross n
let rAPcrossN = rAP[0] * n[1] - rAP[1] * n[0]; // rAP cross n
// Calc impulse scalar, formula of jN
// can be found in http://www.myphysicslab.com/collision.html
let jN = -(1 + newRestituion) * rVelocityInNormal;
jN = jN / (b.getInvMass() + a.getInvMass() +
           rBPcrossN * rBPcrossN * b.getInertia() +
           rAPcrossN * rAPcrossN * a.getInertia());
let rBPcrossT = rBP[0] * tangent[1] - rBP[1] * tangent[0];
let rAPcrossT = rAP[0] * tangent[1] - rAP[1] * tangent[0];
let jT = (newFriction - 1) * rVelocityInTangent;
jT = jT / (b.getInvMass() + a.getInvMass() +
           rBPcrossT * rBPcrossT * b.getInertia() +
           rAPcrossT * rAPcrossT * a.getInertia());
```

## Step F: Update Linear and Angular Velocities

**Step F**: Update linear and angular velocities. These updates follow Equations (5), (6), (19), and (20) exactly.

```javascript
// Update linear and angular velocities
vec2.scaleAndAdd(va, va, n, (jN * a.getInvMass()));
vec2.scaleAndAdd(va, va, tangent, (jT * a.getInvMass()));
setAngularVelocityDelta((rAPcrossN * jN * a.getInertia() +
                         rAPcrossT * jT * a.getInertia()));
vec2.scaleAndAdd(vb, vb, n, -(jN * b.getInvMass()));
vec2.scaleAndAdd(vb, vb, tangent, -(jT * b.getInvMass()));
b.setAngularVelocityDelta(-(rBPcrossN * jN * b.getInertia() +
                            rBPcrossT * jT * b.getInertia()));
```

### Observations

Run the project to test your implementation. The shapes that you insert into the scene now rotate, collide, and respond in fashions that are similar to the real world. A circle shape rolls around when other shapes collide with them, while a rectangle shape should rotate naturally upon collision. The interpenetration between shapes should not be visible under normal circumstances. However, two situations can still cause observable interpenetrations: first, a small relaxation iteration, or second, your CPU is struggling with the number of shapes. In the first case, you can try increasing the relaxation iteration to prevent any interpenetration.

With the rotational support, you can now examine the effects of mass differences in collisions. With their abilities to roll, collisions between circles are the most straightforward to observe. Wait for all objects to be stationary and use the arrow key to select one of the created circles; type the M key with up arrow to increase its mass to a large value, for example, 20. Now select another object and use the WASD key to move and drop the selected object on the high-mass circle. Notice that the high-mass circle does not have much movement in response to the collision. For example, chances are a collision does not even cause the high-mass circle to roll. Now, type the H key to inject random velocities to all objects and observe the collisions. Notice that the collisions with the high-mass circle are almost like collisions with stationary walls/platforms. The inversed mass and rotational inertia modeled by the Impulse Method is capable of successfully capturing the collision effects of objects with different masses.

Now your 2D physics engine implementation is completed. You can continue testing by creating additional shapes to observe when your CPU begins to struggle with keeping up real-time performance.

## Summary

This chapter has guided you through understanding the foundation behind a working physics engine. The complicated physical interactions of objects in the real world are greatly simplified by focusing only on rigid body interactions or rigid shape simulations. The simulation process assumes that objects are continuous geometries with uniformly distributed mass where their shapes do not change during collisions. The computationally costly simulation is performed only on a selected subset of objects that are approximated by simple circles and rectangles.

A step-by-step derivation of the relevant formulae for the simulations is followed by a detailed guide to the building of a functioning system. You have learned to extract collision information between shapes, formulate and compute shape collisions based on the Separating Axis Theorem, approximate Newtonian motion integrals with the Symplectic Euler Integration, resolve interpenetrations of colliding objects based on numerically stable gradual relaxations, and derive and implement collision resolution based on the Impulse Method.

Now that you have completed your physics engine, you can carefully examine the system and identify potentials for optimization and further abstractions. Many improvements to the physics engine are still possible. This is especially true from the perspective of supporting game developers with the newly defined and powerful functionality. For example, most physics engines also support straightforward collision detections without any responses. This is an important missing functionality from your physics component. While your engine is capable of simulating collisions results as is, the engine does not support responding to the simple, and computationally much lower cost, question of if objects have collided. As mentioned, this can be an excellent exercise.

Though simple and missing some convenient interface functions, your physics component is functionally complete and capable of simulating rigid shape interactions with visually pleasant and realistic results. Your system supports intuitive parameters including object mass, acceleration, velocity, restitution, and friction that can be related to behaviors of objects in the real world. Though computationally demanding, your system is capable of supporting a nontrivial number of rigid shape interactions. This is especially the case if the game genre only required one or a small set, for example, the hero and friendly characters, interacting with the rest of the objects, for example, the props, platforms, and enemies.

### Game Design Considerations

The puzzle level in the examples to this point has focused entirely on creating an understandable and consistent logical challenge; we've avoided burdening the exercise with any kind of visual design, narrative, or fictional setting (design elements traditionally associated with enhancing player presence) to ensure we're thinking only about the rules of play without introducing distractions. However, as you create core game mechanics, it's important to understand how certain elements of gameplay can contribute directly to presence; the logical rules and requirements of core game mechanics often have a limited effect on presence until they're paired with an interaction model, sound and visual design, and a setting. As discussed in Chapter [8](#334805_2_En_8_Chapter.xhtml), lighting is an example of a presence-enhancing visual design element that can also be used directly as a core game mechanic, and introducing physics to game world objects is similarly a presence-enhancing technique that's perhaps even more often directly connected to gameplay.
