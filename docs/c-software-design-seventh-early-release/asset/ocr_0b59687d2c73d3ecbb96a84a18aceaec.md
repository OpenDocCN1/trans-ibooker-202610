![](bbox=[0.16, 0.12, 0.84, 0.72])

Figure 5-4. The UML representation of the Strategy design pattern.

The isolation of the drawing aspect now allows us to change the implementation of drawing without having to modify the shape classes. This fulfills the idea of the SRP. You are now also able to introduce new implementations of draw() without modification of any other code. That