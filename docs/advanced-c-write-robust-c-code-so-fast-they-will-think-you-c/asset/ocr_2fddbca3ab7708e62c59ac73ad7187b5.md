6. To fix the style, simply use **Source | Format** and choose to format the entire file. Luckily, the variable names do not have any problems.

7. Simplify the code to make it more understandable. The loop in **calculateAndPrintSpeedData** is doing a couple of things at the same time. It's calculating the speed, finding the minimum and maximum values of it, checking whether we crossed a threshold, and storing the speed. If the speed was a transient value, taking it apart would mean storing it somewhere to loop on it one more time. However, since we are storing it in the speeds array anyway, we can loop one more time on it for clarity of code. Here is the updated version of the loop:

```
for (int i = 0; i < numEntries; ++i) {
    double dt = timesInSeconds[i + 1] - timesInSeconds[i];
    assert(dt > 0);
    double speed = (positions[i + 1] - positions[i]) / dt;
    speeds[i] = speed;
}
```

```
for (int i = 0; i < numEntries; ++i) {
    double speed = speeds[i];
    if (maxSpeed < speed) {
        maxSpeed = speed;
    }

    if (minSpeed > speed) {
        minSpeed = speed;
    }
}
```

```
for (int i = 0; i < numEntries; ++i) {
    double speed = speeds[i];
    double dt = timesInSeconds[i + 1] - timesInSeconds[i];

    if (speed > speedLimit) {
        limitCrossDuration += dt;
    }
}
```