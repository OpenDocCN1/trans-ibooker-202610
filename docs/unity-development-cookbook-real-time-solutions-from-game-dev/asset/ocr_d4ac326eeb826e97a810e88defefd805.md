```
overlappingColliders);

// (Note: we _could_ have used OverlapCapsule, which returns a
// brand-new array, but that requires the function to allocate
// the memory for it on the heap. Because we don't use this
// array after this function ends, the array would turn into
// garbage. More garbage means the garbage collector will run
// more often, which means performance hitches. By creating our
// array locally, we ensure it's stored on the stack; data on
// the stack doesn't get turned into garbage when it goes away,
// but it can't stay around after this function returns,
// which is fine for this case.)

// For each item we were told the capsule overlaps...
for (int i = 0; i < overlapCount; i++) {

    // Get the collider the capsule overlaps
    var overlappingCollider = overlappingColliders[i];

    // If this collider is our controller, ignore it
    if (overlappingCollider == controller) {
        continue;
    }

    // We need to compute how much movement we need to perform
    // to not overlap this collider.

    // First, define some variables to store the direction and
    // distance.
    Vector3 direction;
    float distance;

    // Next, provide information about both our collider and the
    // other one. Our direction and distance variables will be
```