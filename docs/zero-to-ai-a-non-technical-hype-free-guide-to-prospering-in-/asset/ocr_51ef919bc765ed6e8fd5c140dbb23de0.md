content with the sole purpose of keeping subscribers hooked for multiple hours a day. Riskier suggestions, with a correspondingly higher risk of disappointment, are instead underweighted.

Any algorithm used by hundreds of millions of people worldwide can’t be exempt from criticism, and the Netflix recommender system is no exception. While egregiously wrong recommendations often make the rounds on social media, subtler mis-predictions also reveal unsolved problems in the domain. Users frequently complain about the model being unable to keep track of multiple independent “moods” and media consumption tendencies. The typical example is a Friday night trash-TV binge affecting recommendation for the following two months.

# 6.5.3 The business value of recommendations

In 2015, Netflix Chief Product Officer Neil Hunt and Vice President of Product Innovation Carlos Gomez-Urib e wrote “The Netflix Recommender System: Algorithms, Business Value, and Innovation” (https://dl.acm.org/citation.cfm?id=2843948). They reported that the Netflix recommender system was responsible for 80% of the hours streamed on the platform, with the remaining 20% coming from the search functionality. However, because users often search for titles that are not in the catalog, a share of that 20% becomes a recommendation problem.

When evaluating the effectiveness of algorithms, Netflix relies on specific metrics. Two of the most important ones are the effective catalog size (ECS) and the take-rate.

The ECS is a metric that describes how spread out viewing is across the items in the catalog. The metric has a mathematical formulation, but the main idea behind it is rather simple: if most viewing comes from a single video, it will be close to 1. If all videos generate the same amount of viewing, the ECS is close to the number of videos in the catalog. If the catalog has some movies that are rarely seen and some that are more popular, ECS is somewhere in the middle. The higher this value, the more people are watching all the movies in the catalog, which results in higher customer satisfaction and increased return on the money that Netflix spent to either acquire the rights to distribute the media or to produce it. The goal of Netflix is therefore to maximize this value. To test the efficacy of the recommender system, Netflix tried serving content to users following these two approaches:

+   - Using a popularity metric—Starting with the most popular movie in the catalog and gradually adding other popular movies (black line)
+   - Using the personalized system—Starting with the movie ranking first with the PVR score and adding other movies following the order dictated by the PVR algorithm

Using the personalized system resulted in an increase of the ECS of 400%.

The other key engagement metric used by Netflix is the take-rate, defined as the fraction of recommendations offered that results in a play. A value close to 1 means that recommendation will be picked 100% of the time, while a value close to 0 means that the recommendation won’t be picked. Netflix tried proposing users a series of recommendations following two approaches: