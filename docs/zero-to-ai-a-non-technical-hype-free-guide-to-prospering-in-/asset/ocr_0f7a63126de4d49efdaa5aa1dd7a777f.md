in anything you’re interested in and immediately get a list of results sorted by relevance. This task is a branch of NLP often called document search and retrieval.

What we’re trying to do is the following: we want to have a search bar at the top of our website, and when a user types something they’re interested in (let’s say “house insulation”), the algorithm will search through all the available reviews and pick the ones that are the most relevant to their search, as in figure 5.11.

![](bbox=[0.18, 0.24, 0.65, 0.48])

Figure 5.11 In document search, the algorithm is trained on a set of documents (top left). When it receives a query by the user (top right), it produces a list of the most relevant content.

For document search, the concept of word embeddings can help us big time. Recall that a word embedding is a set of a few hundred numbers that encode the meaning of a word, with the property that similar words have similar embeddings. Embeddings allow us to transfer the meaning of similar from the nebulous world of literature into the cold world of numbers. We can take advantage of this so that when the user asks about “house insulation,” we can transform these two words into their embeddings, and if we’ve done the same with all the reviews, we can then look for the reviews whose numerical representation is closer to the one we got for “house insulation.”

We want to help you visualize this concept in your head, so imagine a two-element embedding: each word is represented by only two numbers, as drawn in figure 5.12. Of course, such an embedding won’t be very useful in practice, because two numbers are not nearly enough to capture the richness of vocabulary (the embeddings used in real-world applications are made by hundreds or thousands of numbers).

Imagine drawing a point for each of the documents we have in our database, and then drawing a new point corresponding to the embedding of the user query. Thanks to the way the embeddings are built, the documents most similar to the user query will be physically close to the query point. This means that it’s easy to answer to the user query: just pick a few of the points that are closest to it, and those will correspond to the most similar documents. This solution also works when embedding vectors have more than two numbers; it’s just harder to visualize them.

Document search is a fairly mature application of NLP that often plays the role of a building block in more-complex products. It’s obviously not as fancy as other