
```javascript
function collideShape(s1, s2, infoSet = null) {
    let hasCollision = false;
    if ((s1 !== s2) &&
        ((s1.getInvMass() !== 0) || (s2.getInvMass() !== 0))) {
        if (s1.boundTest(s2)) {
            hasCollision = s1.collisionTest(s2, mCInfo);
            if (hasCollision) {
                ... identical to previous code ...
                positionalCorrection(s1, s2, mCInfo);
                **resolveCollision(s1, s2, mCInfo);**
                ... identical to previous code ... };
```

### Updating MyGame for Testing Collision Resolution

The modifications to the `MyGame` class are trivial, mainly to toggle both motion and positional correction to be active by default. Additionally, initial random rotations of the created `RigidShape` objects are disabled because at this point, collision response does not support rotation. As always, you can refer to the source code files in the `src/my_game` folder for implementation details.

### Observations

You should test your implementation in three ways. First, ensure that moving shapes collide and behave naturally. Second, try changing the physical properties of the objects. Third, observe the collision resolution between shapes that are in motion and shapes that are stationary with infinite mass (the surrounding walls and stationary platforms). Remember that only linear velocities are considered and rotations will not result from collisions.

Now, run the project and notice that the shapes fall gradually to the platforms and floor with their motions coming to a halt after slight rebounds. This is a clear indication that the base case for `Euler Integration`, collision detection, positional correction, and resolution all are operating as expected. Press the `H` key to excite all shapes and the `C` key to display the collision information. Notice the wandering shapes and the walls/platforms interact properly with soft bounces and no apparent interpenetrations.

Use the left/right arrow to select an object and adjust its restitution/friction coefficients with the `N`/`F` and up-/down-arrow keys. For example, adjust the restitution to 1 and friction to 0. Now inject velocity with the `H` key. Notice how the object seems extra bouncy and, with a friction coefficient of 0, seems to skid along platforms/floors. You can try different coefficient settings and observe corresponding bounciness and slipperiness.

The stability of the system can be tested by increasing the number of shapes in the scene with the `G` key. The relaxation loop count of 15 continuously and incrementally pushes interpenetrating shapes apart during each iteration. For example, you can toggle off movement and positional corrections with the `V` and `P` keys and create multiple, for example, 10 to 20, overlapping shapes. Now toggle on motion and positional corrections and observe a properly functioning system.

In the next project, you will improve the resolution solution to consider angular velocity changes as a result of collisions.

## Angular Components of Collision Responses

Now that you have a concrete understanding and have successfully implemented the `Impulse Method` for collision responses with linear velocities, it is time to integrate the support for the more general case of rotations. Before discussing the details, it is helpful to relate the correspondences of Newtonian linear mechanics to that of rotational mechanics. That is, linear displacement corresponds to rotation, velocity to angular velocity, force to torque, and mass to rotational inertia or angular mass. Rotational inertia determines the torque required for a desired angular acceleration about a rotational axis.

The following discussion focuses on integrating rotation in the `Impulse Method` formulation and does not attempt to present a review on Newtonian mechanics for rotation. Conveniently, integrating proper rotation into the `Impulse Method` does not involve the derivation of any new algorithm. All that is required is the formulation of impulse responses with proper consideration of rotational attributes.

### Collisions with Rotation Consideration

The key to integrating rotation into the `Impulse Method` formulation is recognizing the fact that the linear velocity you have been working with, for example, velocity ![$$ {\overrightarrow{\mathrm{V}}}_{\mathrm{A}1} $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq69.png) of object A, is actually the velocity of the shape at its center location. In the absence of rotation, this velocity is constant throughout the object and can be applied to any position. However, as illustrated in Figure [9-28](#334805_2_En_9_Chapter.xhtml#Fig28), when the movement of an object includes angular velocity, ![$$ {\overrightarrow{\upomega}}_{\mathrm{A}1} $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq70.png), its linear velocity at a position P, ![$$ {\overrightarrow{\mathrm{V}}}_{AP1} $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq71.png), is actually a function of the relative position between the point and the center of rotation of the shape or the positional vector ![$$ {\overrightarrow{\mathrm{R}}}_{AP} $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq72.png).

*   ![$$ {\overrightarrow{\mathrm{V}}}_{AP1}={\overrightarrow{\mathrm{V}}}_{\mathrm{A}1}+\left({\overrightarrow{\upomega}}_{\mathrm{A}1}\times {\overrightarrow{\mathrm{R}}}_{AP}\right) $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq73.png)

![Figure 9-28 ¶ Linear velocity at a position in the presence of rotation](images/334805_2_En_9_Chapter/334805_2_En_9_Fig28_HTML.png)

**Figure 9-28 ¶ Linear velocity at a position in the presence of rotation**

> **Note**
>
> Angular velocity is a vector that is perpendicular to the linear velocity. In this case, as linear velocity is defined on the X/Y plane, ![$$ \overrightarrow{\upomega} $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq74.png) is a vector in the z direction. Recall from discussions in the “Introduction” section of this chapter, the very first assumption made was that rigid shape objects are continuous geometries with uniformly distributed mass where the center of mass is located at the center of the geometric shape. This center of mass is the location of the axis of rotation. For simplicity, in your implementation, ![$$ \overrightarrow{\upomega} $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq75.png) will be stored as a simple scalar representing the z-component magnitude of the vector.

Figure [9-29](#334805_2_En_9_Chapter.xhtml#Fig29) illustrates an object B with linear and angular velocities of ![$$ {\overrightarrow{\mathrm{V}}}_{\mathrm{B}1} $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq76.png) and ![$$ {\overrightarrow{\upomega}}_{\mathrm{B}1} $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq77.png) colliding with object A at position P. By now, you know that the linear velocities at point P before the collision for the two objects are as follows:

*   ![$$ {\overrightarrow{\mathrm{V}}}_{AP1}={\overrightarrow{\mathrm{V}}}_{\mathrm{A}1}+\left({\overrightarrow{\upomega}}_{\mathrm{A}1}\times {\overrightarrow{\mathrm{R}}}_{AP}\right) $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq78.png) **(9)**
*   ![$$ {\overrightarrow{\mathrm{V}}}_{BP1}={\overrightarrow{\mathrm{V}}}_{\mathrm{B}1}+\left({\overrightarrow{\upomega}}_{\mathrm{B}1}\times {\overrightarrow{\mathrm{R}}}_{BP}\right) $$](images/334805_2_En_9_Chapter/334805_2_En_9_Chapter_TeX_IEq79.png) **(10)**

![Figure 9-29 ¶ Colliding shapes with angular velocities](images/334805_2_En_9_Chapter/334805_2_En_9_Fig29_HTML.png)

**Figure 9-29 ¶ Colliding shapes with angular velocities**

After the collision, the linear velocity at the collision position can be expressed as follows:
