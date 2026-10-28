Check out the steps highlighted in the preceding image. The state of door #12 has changed at steps 1, 2, 3, 4, 6, and 12. All these steps are divisors of 12. Moreover, step 1 opens the door, step 2 closes the door, step 3 opens the door, step 4 closes the door, step 6 opens the door, and step 12 closes the door. Starting from this observation, we can conclude that for every pair of divisors, the door will just end up back in its initial state, which is closed. In other words, each door that has an even number of divisors remains closed in the end.

Let's see whether this is true for a perfect square, such as 9. The reason for choosing a perfect square relies on the fact that a perfect square always has an odd number of positive divisors. For example, the divisors of 9 are 1, 3, and 9. This means that door #9 remains open.

Based on these two paragraphs, we can conclude that, after 100 visits, the doors that remain opened are those that are perfect squares (#1, #4, #9, #16, ..., #100), while the rest of the doors remain closed.

```
```java
private static final int DOORS = 100;
public static int[] visitToggle() {
	// 0 - closed door
	// 1 - opened door
	int[] doors = new int[DOORS];
	for (int i = 0; i <= (DOORS - 1); i++) {
		doors[i] = 0;
	}
	for (int i = 0; i <= (DOORS - 1); i++) {
		for (int j = 0; j <= (DOORS - 1); j++) {
			if ((j + 1) % (i + 1) == 0) {
				if (doors[j] == 0) {
					doors[j] = 1;
				} else {
					doors[j] = 0;
				}
			}
		}
	}
	return doors;
}
```
```