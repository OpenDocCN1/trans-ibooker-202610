To graphically demonstrate what we are looking to achieve here, check out the diagram below.

![](bbox=[0.116, 0.165, 0.887, 0.745])

**The sequence of events we are looking to implement using AJAX and JQuery.** On the left are client-side events, triggered by the user clicking the Like Category button for the Python category. Following the arrows, you can then work out the events following this.

Study the diagram carefully to help your understanding of what will be implemented. On the left are events that take place client-side (within the browser), with events that take place server-side on the right (within the Django middleware). Everything starts from the user clicking the `Like Category` button. The JQuery code