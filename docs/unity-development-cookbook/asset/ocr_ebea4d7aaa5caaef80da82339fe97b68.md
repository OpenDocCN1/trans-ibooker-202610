methods private to prevent other classes from calling them, since only the engine should be doing this.

![](bbox=[0.17, 0.12, 0.26, 0.22])

You can potentially improve the performance of your frame by removing any unused Update methods from classes. For more information, see the Unity documentation.

By default, Unity doesn’t guarantee the order in which different scripts have their methods run. However, if you really want to ensure that scripts of type A have their Update method run before scripts of type B, you can manually configure their execution order. To do this, open the Edit menu and choose Settings, and then select Script Execution Order. From here, you can manage the execution order of certain scripts, ensuring that they’re in the order that you want (Figure 2-3).

![](bbox=[0.14, 0.36, 0.86, 0.67])

Figure 2-3. Configuring the execution order of some scripts

# 2.3 Creating Frame Rate–Independent Behavior

# Problem

You want to ensure that processes take place over a fixed period of time, regardless of how many frames per second your project is running at (known as the frame rate).