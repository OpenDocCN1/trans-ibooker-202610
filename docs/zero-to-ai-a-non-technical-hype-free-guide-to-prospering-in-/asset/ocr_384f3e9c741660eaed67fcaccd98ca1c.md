![](bbox=[0.175, 0.081, 0.592, 0.376])

Koike decided to try to automate this process so his family could focus on optimizing the cultivation. Thanks to his background in automotive engineering, he was able to design and build the mechanical components for an automated sorting machine based on a conventional conveyor belt design.

Automated arms (controlled by a computer) push each cucumber into one of the bins based on its quality classification. Once the machinery was built and tested, Koike had to implement the control algorithm for the arms. From the point of view of implementation, the hardest tasks to complete are as follows:

+   1. Identifying distinct items as they move through the conveyor belt
+   2. Classifying each item based on shape, color, surface appearance
+   3. Coordinating the movement of the arms to push each item into the correct bin

Koike could leverage his expertise to tackle tasks 1 and 3, which fall into the realm of conventional automation engineering. He then recognized that task 2 is an *object classification* task, for which he could use a vast amount of research and open source code. This reduced the conventional software engineering effort to the few hours needed to set up one of these freely available frameworks, and he was off to the next real challenge, collecting training data. As often happens in domain-specific situations like this, the only avenue for data collection is to fall back on the only humans that have the necessary expertise. Koike collected examples (pictures) and labels as his mother patiently classified around 8,000 cucumbers according to the farm’s stringent quality criteria. The dataset is freely available at https://github.com/workpiles/CUCUMBER-9.

After enough training data for a prototype has been collected, what remains is just routine software engineering: plugging the predictions of the model into the control system for the conveyor belt. Figure 4.19 illustrates the process: a camera takes an