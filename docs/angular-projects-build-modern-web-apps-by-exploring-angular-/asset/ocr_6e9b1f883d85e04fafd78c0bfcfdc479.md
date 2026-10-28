If we now run our application using Nx Console, visit some POI from the map, and switch to the http://localhost:4200/admin URL, we should see the following output:

![](bbox=[0.2, 0.17, 0.8, 0.65])

Figure 8.21 – POI statistics

An administrator can now have a complete overview of how each POI is doing from a visit perspective. Our administrator portal has now been completed. The visitor portal can interact with the store and save visit statistics for each POI in the local storage of the browser. The administrator portal can then fetch that data and display it on a pie chart using the ng2-charts library.