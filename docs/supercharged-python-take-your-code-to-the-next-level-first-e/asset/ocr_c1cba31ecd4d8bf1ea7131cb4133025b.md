Figure 3.4 illustrates the concept. Shallow copying makes new copies of top-level data only.

![](bbox=[0.296, 0.158, 0.674, 0.534])

# Figure 3.4. Shallow copying

And now you can see the problem. A member-by-member copy was carried out, but the list within the list was a *reference*, so both lists ended up referring to the same data in the final position.

The solution is simple. You need to do a *deep copy* to get the expected behavior. To get a deep copy, in which even embedded list items get copied, import the **copy** package and use **copy.deepcopy**.

Click here to view code image

```
import copy
```