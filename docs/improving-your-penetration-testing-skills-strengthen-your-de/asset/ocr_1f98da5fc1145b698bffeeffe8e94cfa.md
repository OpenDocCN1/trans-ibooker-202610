Burp Intruder has four different attack types that describe how the inputs will be filled with the payloads:

+   - **Sniper:** This uses a single payload set, and selects each input position, one at a time, for every value within this payload set. The number of requests will be the length of the payload set multiplied by the number of input positions.
+   - **Battering ram:** This uses a single payload set, and selects all input positions simultaneously for every value within this payload set. The number of requests will be the length of the payload set.
+   - **Pitchfork:** This uses multiple input positions, and it requires a payload set for each position. It submits one value for each payload set in its corresponding input at a time. The number of requests made will be the length of the shortest payload set.
+   - **Cluster bomb:** When using multiple inputs, all of the elements in the payload set 1 will be paired with all of the elements of the payload set 2 and so on until the payload set n. The number of requests made in the attack is determined by multiplying all payload sets' sizes.

![](bbox=[0.14, 0.35, 0.22, 0.48])

Next, change to the **Payloads** tab inside Intruder. Leave **Payload set** unchanged, and click on **Load...** in the **Payload Options [Simple List]** section; this is designed to load a file containing the names that you want to try. Luckily, Kali Linux includes an extensive collection of dictionaries and wordlists in the `/usr/share/wordlists` directory.