```
// them. We'll cover this in another recipe!)
controller.Move(moveDirection * Time.deltaTime);
}
}
```

+   4. Attach a Movement component to your Player object.

+   5. Play the game. Pressing the arrow keys or the W, A, S, and D keys will move the player around (unless you’ve customized your input system). You can also jump by pressing the space bar.

# Discussion

It’s certainly possible to implement a completely physically correct control setup for your game, in which the player is simulated as a rigidbody and moves around by having forces applied to that body. However, for the best-feeling control in your game, it’s better to manage the player’s movement using your own code, rather than doing it in physics. This is because it’s very challenging to create a control scheme that responds to player input in a way that feels responsive and pleasant.

Some of the games with the best-feeling controls have extremely unrealistic physics, and even games that claim realism aren’t physically realistic. Every game in the Mario series of platform games has extremely unrealistic physics, but that doesn’t matter when the goal is enjoyable gameplay. Modern action-adventure games like the Tomb Raider series also feature physically implausible movement. It doesn’t matter; what matters is the interaction between the player and your game’s controls. To that end, directly controlling the player’s speed and inertia, rather than having the physics system handle it for you, gives you the control you need over the player’s experience.