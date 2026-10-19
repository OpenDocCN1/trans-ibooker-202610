```
function recursiveMicrotask() {
  Promise.resolve().then(() => {
    // Do some work
    recursiveMicrotask(); // Immediately queue another microtask
  });
}

recursiveMicrotask(); // This will block the browser indefinitely
```

Since each microtask queues another before the tick completes, the browser never gets a chance to render or handle user input. To avoid this, break work into macrotasks using setTimeout or the scheduler API to yield control back to the browser periodically. The scheduler API’s priorities mainly apply to macrotasks. Microtasks always run immediately next unless you break out of them, so avoid unbounded microtask loops.

# The RAIL Model

You met RAIL (response, animation, idle, load) in Chapter 1. Here, you will just use it as a lens for how microtasks and scheduled work affect the main thread.

Response and animation are where long microtask chains can really hurt you. Work that runs in microtasks after an input event still counts against the same response window, and microtasks scheduled inside requestAnimationFrame run before paint. So keep per-interaction work small (to help stay within the INP budget) and avoid piling heavy microtasks into frames that need to render.

Idle is where you can safely move nonurgent work that would otherwise block inputs or animation. The requestIdleCallback API lets you schedule this kind of work so it only runs when the main thread has time:

```
const workQueue = [];
```