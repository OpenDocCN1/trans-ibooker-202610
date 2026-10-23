used to power computers can’t readily be optimized by an operations team because it depends on the computing workload and the efficiency of the chips. For this reason, data center engineers strive to reduce all extra consumption.

The efficiency of data centers is usually measured by tracking a metric called *power usage effectiveness (PUE)*. This metric reflects how much energy is used on anything other than the actual computers that make the data center:

PUE = Total Facility Energy / IT Equipment Energy

A perfect data center has a PUE of 1: all the energy is spent to power the computers. The higher the PUE, the more energy is spent on other systems, among which cooling is the most important. For example, a PUE of 1.5 means that for every kilowatt-hour (kWh) of energy consumed to power computers, an additional 0.5 kWh of energy is needed for cooling and other minor needs.

Google has always been a leader in PUE efficiency. According to a 2018 survey of 900 DC operators by the Uptime Institute, the average PUE in the industry was 1.58. Google continuously improved its PUE until it reached 1.12 in 2013. Unfortunately, this value didn’t improve until 2017.

# THE ML APPROACH TO DATA CENTER EFFICIENCY

Gao realized that one of the obstacles to lowering the PUE further was that it’s extremely complex to predict it correctly in different scenarios using a traditional engineering approach, because of the complex interactions between factors (for instance, wind can help cool the plant and reduce the need for artificial cooling). On the other hand, he was well aware of the large datasets collected by his team as part of day-to-day operations, thanks to thousands of sensors deployed across components that collect millions of data points.

Gao foresaw the potential of using this data to train an ML model capable of overcoming the limitations of traditional thermodynamics. His first approach was to build a simple neural network (a classic algorithm used to build supervised learning models) that was trained to predict the PUE, given a list of features that affect it. Gao used a total of 19 features, including these:

+   - Total server IT load
+   - Total number of process water pumps running
+   - Mean cooling tower leaving-water-temperature set point
+   - Total number of chillers running
+   - Mean heat exchanger approach temperature
+   - Outside air wet-bulb and dry-bulb temperatures
+   - Outside air relative humidity, wind speed, and direction

The label of this supervised learning problem is the PUE. As you can see in figure 2.6, to train his model, Gao used 184,435 time samples at five-minute resolution (approximately two years of operational data). The final model was able to predict DC PUE