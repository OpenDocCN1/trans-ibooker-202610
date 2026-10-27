|  |  |
|---|---|---|
|2045–4092|12 (or /20)|
|4093–8188|13 (or /19)|

You can use a single subnet for multiple GKE clusters.

# Pod IP CIDR

In order to determine the Pod IP CIDR, determine the maximum number of Pods per Node you need in your cluster over its lifetime. If you cannot determine the maximum number you need, use the quota limit of 110 Pods per Node as the maximum. Use the following table to determine the host bits needed for the required number of Pods.

|Pods-per-Node count|Host bits for Pods|
|---|---|
|1–8|4|
|9–16|5|
|17–32|6|
|33–64|7|
|65–110|8|

To calculate the Pod IP CIDR block, you need the host bits for nodes and