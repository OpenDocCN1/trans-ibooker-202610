perform the change, it is necessary to change the behavior in multiple, unrelated places.

But there is more: since the drawing functionality is implemented inside `Circle` and `Square`, the `Circle` and `Square` classes depend on the implementation details of `draw()`, meaning they depend on OpenGL. Despite the fact that circles and squares should primarily be some simply geometric primitives, these two classes now carry the burden of having to use OpenGL everywhere they are used.

When pointing this out to your colleagues, they are, at first, a little dumbfounded. And also a little annoyed, since they did not expect you to point out any flaw in their beautiful solution. However, you have a very nice way of explaining the problem and so eventually, they agree with you and start to think about a better solution.

It does not take them long to come up with a better approach. In the next team meeting a few days later, they present their new idea: another layer in the inheritance hierarchy (see Figure 5-2).