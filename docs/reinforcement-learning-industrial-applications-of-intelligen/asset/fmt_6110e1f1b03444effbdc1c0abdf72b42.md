
Deep Q-networks was the breakthrough paper, but neural networks have been used in RL for a long time.^([22](#ch04.html#idm46428617352568)) Given the flexibility of neural networks, you can find as many improvements to DQN as the number of papers on deep learning. The key insight is that although nonlinear function approximators are unruly and may not converge, they have the incredible ability to approximate any function. This opens the door to applications that were previously deemed too complex.

This power was demonstrated by playing Atari games, but you can see more examples of industrial use cases every day. This chapter marks the end of your journey through basic tabular methods. In the next chapter I reformulate the problem of finding an optimal policy.

## Further Reading

### Machine learning

*   Provost, Foster, and Tom Fawcett. 2013. [*Data Science for Business: What You Need to Know about Data Mining and Data-Analytic Thinking*](https://oreil.ly/gcJ09). O’Reilly Media.
*   Raschka, Sebastian. 2015. *Python Machine Learning*. Packt Publishing Ltd.

### Deep learning

*   Goodfellow, Ian, Yoshua Bengio, and Aaron Courville. 2016. *Deep Learning*. MIT Press.
*   Chollet, François. 2017. *Deep Learning with Python*. Manning Publications Company.

### Generative models (autoencoders and such)

*   Foster, David. 2019. *Generative Deep Learning: Teaching Machines to Paint, Write, Compose, and Play*. O’Reilly Media.

## References

^([1](#ch04.html#idm46428615774280-marker)) Tsitsiklis, J. N., and B. Van Roy. 1997. [“An Analysis of Temporal-Difference Learning with Function Approximation”](https://oreil.ly/aK17s). *IEEE Transactions on Automatic Control* 42 (5): 674–90.

^([2](#ch04.html#idm46428615771240-marker)) Mnih, Volodymyr, Koray Kavukcuoglu, David Silver, Alex Graves, Ioannis Antonoglou, Daan Wierstra, and Martin Riedmiller. 2013. [“Playing Atari with Deep Reinforcement Learning”](https://oreil.ly/HOqeN). ArXiv:1312.5602, December.

^([3](#ch04.html#idm46428615761928-marker)) Zhang, Shangtong, and Richard S. Sutton. 2018. [“A Deeper Look at Experience Replay”](https://oreil.ly/qnRKD). ArXiv:1712.01275, April.

^([4](#ch04.html#idm46428615759736-marker)) Isele, David, and Akansel Cosgun. 2018. [“Selective Experience Replay for Lifelong Learning”](https://oreil.ly/Wsk1d). ArXiv:1802.10269, February.

^([5](#ch04.html#idm46428615752136-marker)) Mnih, Volodymyr, et. al 2018. [“Human-Level Control through Deep Reinforcement Learning”](https://oreil.ly/AU1td). *Nature* 518, 529–533.

^([6](#ch04.html#idm46428615664872-marker)) [“Shedding Light on Energy on the EU: How Are Emissions of Greenhouse Gases by the EU Evolving?”](https://oreil.ly/yLgeU). n.d. Shedding Light on Energy on the EU. Accessed 12 September 2020.

^([7](#ch04.html#idm46428615663096-marker)) Marantos, Charalampos, Christos P. Lamprakos, Vasileios Tsoutsouras, Kostas Siozios, and Dimitrios Soudris. 2018. [“Towards Plug&Play Smart Thermostats Inspired by Reinforcement Learning”](https://oreil.ly/ozLFF). *In Proceedings of the Workshop on INTelligent Embedded Systems Architectures and Applications*, 39–44. INTESA 2018. New York, NY, USA: Association for Computing Machinery.

^([8](#ch04.html#idm46428615648664-marker)) Hessel, Matteo, Joseph Modayil, Hado van Hasselt, Tom Schaul, Georg Ostrovski, Will Dabney, Dan Horgan, Bilal Piot, Mohammad Azar, and David Silver. 2017. [“Rainbow: Combining Improvements in Deep Reinforcement Learning”](https://oreil.ly/wKDL4). ArXiv:1710.02298, October.

^([9](#ch04.html#idm46428615622728-marker)) Bellemare, Marc G., Will Dabney, and Rémi Munos. 2017. [“A Distributional Perspective on Reinforcement Learning”](https://oreil.ly/ODa3y). ArXiv:1707.06887, July.

^([10](#ch04.html#idm46428615600360-marker)) Schaul, Tom, John Quan, Ioannis Antonoglou, and David Silver. 2015. [“Prioritized Experience Replay”](https://oreil.ly/0NPIN). ArXiv:1511.05952, November.

^([11](#ch04.html#idm46428615592376-marker)) Fortunato, Meire, Mohammad Gheshlaghi Azar, Bilal Piot, Jacob Menick, Ian Osband, Alex Graves, Vlad Mnih, et al. 2017. [“Noisy Networks for Exploration”](https://oreil.ly/TNcdO). ArXiv:1706.10295, June.

^([12](#ch04.html#idm46428615575976-marker)) Wang, Ziyu, Tom Schaul, Matteo Hessel, Hado van Hasselt, Marc Lanctot, and Nando de Freitas. 2015. [“Dueling Network Architectures for Deep Reinforcement Learning”](https://oreil.ly/w4xRr). ArXiv:1511.06581, November.

^([13](#ch04.html#idm46428617404008-marker)) Osband, Ian, Charles Blundell, Alexander Pritzel, and Benjamin Van Roy. 2016. [“Deep Exploration via Bootstrapped DQN”](https://oreil.ly/RmkiV). ArXiv:1602.04621, July.

^([14](#ch04.html#idm46428617400232-marker)) Lu, Xiuyuan, and Benjamin Van Roy. 2017. [“Ensemble Sampling”](https://oreil.ly/7Bo99). ArXiv:1705.07347, November.

^([15](#ch04.html#idm46428617398616-marker)) Chen, Richard Y., Szymon Sidor, Pieter Abbeel, and John Schulman. 2017. [“UCB Exploration via Q-Ensembles”](https://oreil.ly/c2e4v). ArXiv:1706.01502, November.

^([16](#ch04.html#idm46428617396888-marker)) Menon, Rakesh R., and Balaraman Ravindran. 2017. [“Shared Learning: Enhancing Reinforcement in Q-Ensembles”](https://oreil.ly/dYjc7). ArXiv:1709.04909, September.

^([17](#ch04.html#idm46428617394696-marker)) Jain, Siddhartha, Ge Liu, Jonas Mueller, and David Gifford. 2019. [“Maximizing Overall Diversity for Improved Uncertainty Estimates in Deep Ensembles”](https://oreil.ly/Ounz8). ArXiv:1906.07380, June.

^([18](#ch04.html#idm46428617388056-marker)) Dabney, Will, Mark Rowland, Marc G. Bellemare, and Rémi Munos. 2017. [“Distributional Reinforcement Learning with Quantile Regression”](https://oreil.ly/pLAuN). ArXiv:1710.10044, October.

^([19](#ch04.html#idm46428617385752-marker)) Lyle, Clare, Pablo Samuel Castro, and Marc G. Bellemare. 2019. [“A Comparative Analysis of Expected and Distributional Reinforcement Learning”](https://oreil.ly/jY6-f). ArXiv:1901.11084, February.

^([20](#ch04.html#idm46428617370584-marker)) Fujimoto, Scott, David Meger, and Doina Precup. 2019. [“Off-Policy Deep Reinforcement Learning without Exploration”](https://oreil.ly/6EdcI). ArXiv:1812.02900, August.

^([21](#ch04.html#idm46428617359240-marker)) Shah, Devavrat, and Qiaomin Xie. 2018. [“Q-Learning with Nearest Neighbors”](https://oreil.ly/1VJH5). ArXiv:1802.03900, October.

^([22](#ch04.html#idm46428617352568-marker)) Gaskett, Chris, David Wettergreen, and Alexander Zelinsky. 1999. [“Q-Learning in Continuous State and Action Spaces”](https://oreil.ly/jMjWm). In *Advanced Topics in Artificial Intelligence*, edited by Norman Foo, 417–28. Lecture Notes in Computer Science. Berlin, Heidelberg: Springer.

# Chapter 5. Policy Gradient Methods

[Chapter 2](#ch02.html#ch_mdp-dp-mc) introduced value methods, which allow agents to learn the expected return by visiting each state-action pair. The agent can then choose the most valuable action at each time step to maximize the reward. You saw three different ways of estimating the expected return—Monte Carlo, dynamic programming, and temporal-difference methods—but all attempt quantify the value of each state.

Think about the problem again. Why do you want to learn expected values? They allow you to iterate toward an optimal policy. Q-learning finds the optimal policy by repeatedly maximizing the next expected value. But this is an indirect route to a policy. Is it possible to find the optimal policy directly? The answer is yes. Policy-based methods allow the agent to select actions without consulting a value function and learn an optimal policy directly.

## Benefits of Learning a Policy Directly
