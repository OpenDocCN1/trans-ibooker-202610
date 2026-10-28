# Step 4 Now, with the DNSSEC option back, let’s try a resource record that doesn’t exist. Enter the following command:

```
dig +dnssec rit.arin.net.
```

Again, there is an NXDOMAIN status, but now you’ll notice a Next SECure (NSEC) resource record that has ripe.arin.net. on the left and rpki.arin.net. on the right. The string rit actually falls right in the middle, alphabetically, between ripe and rpki, as shown in Figure 7-14.

![](bbox=[0.186, 0.295, 0.813, 0.717])

FIGURE 7-14 There is no rit.arin.net (with proof).

# Step 5 When we try another name, we again get confirmation that the resource record doesn’t exist, and again we are also told what legitimate resource records it falls between. Enter the following command:

```
dig +dnssec jonathan.arin.net.
```