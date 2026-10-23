![](bbox=[0.15, 0.11, 0.82, 0.35])

Figure 4.21 The upgraded version of Makoto’s dataset, which results from each cucumber being photographed from three different angles

Koike’s story has fascinated many news outlets, which reported on this David versus Goliath story indicating that even a single engineer can build AI applications that rival those of big corporations. Many of these reporters failed to mention that Koike took three months to collect the 8,000 samples needed for the prototype. The images he captured were also of such an extremely low resolution that it’s physically impossible for the model to recognize prickles and scratches. On the other hand, Google’s blog released its own version of the story, which emphasized the role of its open source libraries and the cloud services it offers to speed up training.

# 4.8.1 Case questions

+   1. Why is this problem a good fit for machine learning?
+   2. What key choices and trade-offs allowed Koike to quickly develop a prototype with minimal investment?
+   3. How would you design guidelines for measuring the success of this project?

# 4.8.2 Case discussion

The business case for automating production processes has been clear ever since the time of the Industrial Revolution. This project is an example of AI bringing about a new wave of improvements by focusing on advanced perception for robotics. Many plants are designed so that industrial robots can complete tasks with a minimum need for flexibility: think about a conveyor belt carrying open cans while they’re being filled with a product. In a case like that, the algorithms that control the plant have little need for cameras or other advanced sensors, because the process is strict. The situation with the cucumbers is dramatically different, as it’s hard to control what kind of