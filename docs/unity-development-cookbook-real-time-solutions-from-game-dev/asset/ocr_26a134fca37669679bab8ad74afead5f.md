![](bbox=[0.142, 0.088, 0.855, 0.206])

*Figure 5-8. A ConstantForce2D component*

# ## 5.9 Creating a Conveyor Belt

# ### Problem

You want to create an object that pushes around any object that touches it.
For example, you want to create a conveyor belt.

# ### Solution

Surface effectors apply forces that push bodies along the edges of the
effector. To create a conveyor belt:

+   1. Create a new game object, and add a `BoxCollider2D` component to it. Name it “Conveyor Belt.”
+   
+   2. Turn on the Used By Effector setting (Figure 5-9).
+   
+   3. Add a `SurfaceEffector2D` component by clicking the Add
+   Component button and choosing Physics 2D → Surface Effector 2D
+   (Figure 5-10).
+   
+   4. Place above it any object with a `Rigidbody2D` and a `Collider2D`
+   attached.
+   
+   5. Run the game. The object will fall onto the effector and be pushed by
+   it.