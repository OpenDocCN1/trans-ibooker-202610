```
RaycastHit hit;

// The maximum distance we want to look for.
float maxDistance = (controller.height / 2f) + 0.1f;

// Cast the ray. Did it hit anything?
if (Physics.Raycast(ray, out hit, maxDistance)) {

  // It did!

  // Did it have a MovingPlatform component?
  var platform = hit.collider.gameObject
    .GetComponent<MovingPlatform>();

  if (platform != null) {
    // If it did, update our position based on the
    // platform's current velocity.
    transform.position +=
      platform.velocity * Time.deltaTime;
  }
}
}
}
}
```

+   2. Add a PlatformRiding component to the player character—that is, the object that has your CharacterCollider.

+   3. Play the game, and jump onto an object that has a MovingPlatform component. Your character will be carried with the platform. Additionally, if you get off the platform and then stand in its way, you will be pushed by the platform as it moves into you.

# Discussion