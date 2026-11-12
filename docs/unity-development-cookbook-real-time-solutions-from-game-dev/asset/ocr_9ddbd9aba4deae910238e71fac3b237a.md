3. Attach a Rigidbody component to the sphere. Disable gravity on the Rigidbody.

4. Attach a DamageGiver component to the sphere.

5. Drag the sphere from the Hierarchy into the Project tab. This will create a prefab.

6. Delete the original sphere from the scene.

7. Create a cylinder, and name it “Shooter.”

8. Attach the ProjectileShooter.cs script to the cylinder.

9. Drag the Projectile prefab into the cylinder’s Projectile Prefab field.

10. Position and orient the cylinder so that its forward direction—that is, its blue arrow—is aimed at the Damage Receiver.

11. Start the game. The shooter will fire spheres at the target. After enough of them hit it, the target will be removed from the scene.

# Discussion

Our hit point system is only one possible way this behavior could be implemented. You could globally keep track of all objects with hit points in a grand central list of some kind and manage it there, for example.

> NOTE
> 
> Repeatedly instantiating objects that you know will be destroyed later can be inefficient, because you’re continuously allocating and freeing memory. We’re doing it in this recipe for the sake of keeping the demo simple, but for a real game, consider using an object pool, which we discuss in Recipe 2.8.

# 9.3 Creating a Top-Down Camera

# Problem