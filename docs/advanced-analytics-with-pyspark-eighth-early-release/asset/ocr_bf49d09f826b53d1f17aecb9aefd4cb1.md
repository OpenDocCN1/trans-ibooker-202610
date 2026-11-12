practitioners to articulate what the numbers mean when they make predictions.

It seems fitting to kick off the next three chapters, which will explore key machine learning algorithms on PySpark, with a chapter built around recommender engines, and recommending music in particular. It’s an accessible way to introduce real-world use of PySpark and MLlib, and some basic machine learning ideas that will be developed in subsequent chapters.

In this chapter, we will implement a recommender system in PySpark. Specifically, we will use the Alternating Least Squares (ALS) algorithm on an open dataset provided by a music streaming service. We will start off by understanding the dataset and importing it in PySpark. We will then discuss our motivation for choosing the ALS algorithm and its implementation in PySpark. This will be followed by data preparation and building our model using PySpark. We will finish off by making some user recommendations and discussing ways to improve our model through hyperparameter selection.

# # Setting up the Data

We will will use a data set published by Audioscrobbler. Audioscrobbler was the first music recommendation system for last.fm, one of the first internet streaming radio sites, founded in 2002. Audioscrobbler provided an open API for “scrobbling,” or recording listeners’ song plays. last.fm used this information to build a powerful music recommender engine. The system reached millions of users because third-party apps and sites could provide listening data back to the recommender engine.

At that time, research on recommender engines was mostly confined to learning from rating-like data. That is, recommenders were usually viewed as tools that operated on input like “Bob rates Prince 3.5 stars.” The Audioscrobbler data set is interesting because it merely records plays: “Bob played a Prince track.” A play carries less information than a rating. Just because Bob played the track doesn’t mean he actually liked it. You or I