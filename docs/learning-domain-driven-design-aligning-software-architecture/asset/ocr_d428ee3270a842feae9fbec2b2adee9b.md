three groups, each representing a type of team collaboration: cooperation, customer–supplier, and separate ways.

# Cooperation

Cooperation patterns relate to bounded contexts implemented by teams with well-established communication.

In the simplest case, these are bounded contexts implemented by a single team. This also applies to teams with dependent goals, where one team’s success depends on the success of the other, and vice versa. Again, the main criterion here is the quality of the teams’ communication and collaboration.

Let’s look at two DDD patterns suitable for cooperating teams: the partnership and shared kernel patterns.

# Partnership

In the partnership model, the integration between bounded contexts is coordinated in an ad hoc manner. One team can notify a second team about a change in the API, and the second team will cooperate and adapt—no drama or conflicts (see Figure 4-1).

![](bbox=[0.135, 0.62, 0.88, 0.77])

Figure 4-1. The partnership model

The coordination of integration here is two-way. No one team dictates the language that is used for defining the contracts. The teams can work out the differences and choose the most appropriate solution. Also, both sides cooperate in solving any integration issues