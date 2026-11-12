All three of these methods receive a Collision parameter, which stores information about the collision, including the speed at which the collision hit, and the point in the world at which the objects are making contact.

NOTE
Collisions will be detected only when at least one of the two colliding objects has a nonkinematic rigidbody. A kinematic rigidbody is one that applies forces to other objects, but does not respond to forces itself. For example, a floating platform that doesn’t respond to gravity could be set up using a kinematic body. To make a body kinematic, check the Is Kinematic box in the Inspector (see Figure 7-1).

![](bbox=[0.15, 0.375, 0.85, 0.655])

Figure 7-1. Enabling the Is Kinematic option on a rigidbody

# 7.7 Detecting When an Object Is in a Trigger Area

# Problem

You want to detect when an object has entered a collider that’s marked as a trigger.