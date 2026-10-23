# 3.3 Using Input Actions

# Problem

You want to set up Input Actions, which map physical inputs to in-game actions.

# Solution

In the Unity Input System, an Input Action is an in-game action that the player can perform in the game (for example, moving, jumping, turning) that's represented by one or more physical controls that the user can interact with (for example, the W key on a keyboard, or the left thumbstick on a gamepad).

Input Actions allow you to think in terms of the higher-level things that happen in your game, rather than having to think at the low level of pressing specific buttons. More importantly, because you can bind multiple controls to a single action, it makes it a lot easier to build a game that targets a variety of control schemes.

![](bbox=[0.165, 0.455, 0.25, 0.525])

Before following the steps in this recipe, make sure you've installed the Unity Input System in your project by following the steps in Recipe 3.2.

To demonstrate Input Actions, we'll start by creating a simple little scene that can be controlled by user input:

+   1. In a new empty scene, create a new cube by opening the GameObject menu and choosing 3D Object → Cube.
+   2. Select the newly created cube, and in the Inspector, click Add Component. Type CubePlayer, click “New script,” and then click Create and Add.
+   3. Open the newly created file, CubePlayer.cs, and replace all of its contents with the following:

```
using UnityEngine.InputSystem;

public class CubePlayer : MonoBehaviour
{
    [SerializeField] float moveSpeed = 5;
    private Vector2 moveInput = Vector2.zero;

    public void Update() {
        var movement = new Vector3(moveInput.x, moveInput.y, 0)
            * moveSpeed
            * Time.deltaTime;
```