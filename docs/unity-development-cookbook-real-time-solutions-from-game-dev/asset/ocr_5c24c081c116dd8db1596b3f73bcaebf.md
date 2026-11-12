```
{
    // Notify the script that it was just created
    notifier.OnCreatedOrDequeuedFromPool(true);
}

    // Return the object we just created.
    return newObject;
}
}
```

```
// Disables an object and returns it to the queue for later reuse.
public void ReturnObject(GameObject gameObject)
{
    // Find any component that we need to notify
    var notifiers = gameObject
        .GetComponents<IObjectPoolNotifier>();

    foreach (var notifier in notifiers)
    {
        // Let it know that it's returning to the pool
        notifier.OnEnqueuedToPool();
    }

    // Disable the object and make it a child of this one, so that
    // it doesn't clutter up the Hierarchy
    gameObject.SetActive(false);
    gameObject.transform.parent = this.transform;

    // Put the object into the inactive queue
    inactiveObjects.Enqueue(gameObject);
}
}
```