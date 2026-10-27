# Figure 6.92 System alerts, part 1.

```
W19: SOH for NODE: 51 IS 99.8663 PERCENT
I44: RUL IS 4776.1 [hr] AT DATA TIME: 504 [hr]
I43: RECEIVED RESPONSE: CONTINUE
W19: SOH for NODE: 62 IS 99.9713 PERCENT
I44: RUL IS 4795.8 [hr] AT DATA TIME: 960 [hr]
I43: RECEIVED RESPONSE: CONTINUE
W19: SOH for NODE: 61 IS 99.8078 PERCENT
I44: RUL IS 4695.3 [hr] AT DATA TIME: 1104 [hr]
I43: RECEIVED RESPONSE: CONTINUE
W19: SOH for NODE: 50 IS 99.9087 PERCENT
I44: RUL IS 4786.1 [hr] AT DATA TIME: 1368 [hr]
I43: RECEIVED RESPONSE: CONTINUE
W19: SOH for NODE: 61 IS 74.4394 PERCENT
I44: RUL IS 1312.3 [hr] AT DATA TIME: 1536 [hr]
I43: RECEIVED RESPONSE: CONTINUE
W19: SOH for NODE: 51 IS 74.4327 PERCENT
I44: RUL IS 2114.7 [hr] AT DATA TIME: 1224 [hr]
I43: RECEIVED RESPONSE: CONTINUE
```

system. For example, the customer might wish all of the following to be supported as responses to alerts:

+   - CONTINUE operation.
+   - SUSPEND operation.
+   - SCHEDULE maintenance, removal and replacement, and inspection.
+   - MONITOR: update/change sampling rate and/or alert levels.
+   - OFFLINE: end operation.
+   - ONLINE: resume operation of a suspended or offline prognostic target.
+   - NODE: add, update, or remove node definitions (NDEFs) for a system.

Variable sampling. You are likely to discover that each set of LRUs has different sampling requirements based on how and where it is used. For example, an EMA used to adjust the seat in a cockpit is likely to be less critical compared to that used to position a wing surface.

Prognostic Analysis program. Even though you might believe the design of this program is primarily in your zone of expertise, you are likely to find that your customer has other ideas. For example, should the program analyze the specifics for an alpha accuracy of 5%, 10%, and 25%? Your customer might want your program to do any or all of the following:

+   - Analyze and display the accuracy statistics for a prognostic target as specified by PGALPHA (refer back to Figure 6.62).
+   - Analyze and display the estimated accuracy of the estimated RUL values when alerts are issued.
+   - Plot measurements and metrics only when specifically asked for.

Visual services. The design of a GUI for a PHM system is probably the most difficult, time-consuming, and costly aspect of the system, especially because of the following:

+   - The look and feel you prefer may be disliked by your customer.
+   - The GUI must be compatible with and integrate with existing diagnostic and/or prognostic solutions.
+   - The breadth and depth of the functionality need to cover all aspects of the design system: definitions, programs, operational specifications, and so on. Further, every instance where you add, delete, or update functionality is likely to require design, review, testing, and approval costs and time.