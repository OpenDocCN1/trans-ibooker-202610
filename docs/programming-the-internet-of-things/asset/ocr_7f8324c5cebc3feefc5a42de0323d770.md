Figure 1-3. - Notional IoT simplified logical architecture with Edge and Cloud Tiers

Let’s dig into each a bit further:

+   - Constrained Device App (CDA)
+     You’ll build this software application to run within the constrained device (emulated or actual), and it will provide data collection and system triggers functionality. It will handle the interface between the device’s sensors (which read data from the environment) and actuators (which trigger actions, such as turning the thermostat on or off). It will also play a role in taking action when an actuation is needed. Eventually, it will be connected to a communications library to send messages to, and receive messages from, the Gateway Device App.
+   - Gateway Device App (GDA)
+     You’ll build this software application to run within the gateway device (emulated or actual), and it will provide data management, analytics, and configuration management functionality. It’s primary role is to manage data and the connections between the CDA and cloud services that exist within the Cloud Tier. It will manage data locally as appropriate, and - sometimes - take action by sending a command to the constrained device that triggers an actuation. It will also manage some of the configuration settings - that is, those that represent nominal for your environment, and perform some simple analytics when new telemetry is received.
+   - Cloud Services