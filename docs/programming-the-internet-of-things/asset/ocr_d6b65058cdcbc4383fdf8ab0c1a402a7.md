+   - Create a new Java package in the programmingtheiot\gda source folder named system and navigate to that folder.
+   - Import the java.util.logging logging framework. You can import all, or just Level and Logger.
+   - Create a private variable as follows: private int pollSecs = 60;
+   - Create a public constructor that accepts a single parameter as follows: public SystemPerformanceManager(int pollSecs)
+   - Set this.pollSecs to pollSecs only after validating that pollSecs is more than 1 and less than Integer.MAX. You may choose other constraints if you wish.
+   - Add the startManager() method, and log an info message indicating manager was started.
+   - Add the stopManager() method, and log an info message indicating the manager was stopped.

# Estimate (Small = < 2 hrs ; Medium = 4 hrs ; Large = 8 hrs )

+   - Small

# Tests

+   - Run the SystemPerformanceManagerTest unit test named testRunManager(). The log output should look similar to the following:
+   ```
+   Jul 19, 2020 1:01:38 PM
+   programmingtheiot.gda.system.SystemPerformanceManager
+       startManager
+   INFO: SystemPerformanceManager is starting...
+   Jul 19, 2020 1:01:38 PM
+   programmingtheiot.gda.system.SystemPerformanceManager
+   ```