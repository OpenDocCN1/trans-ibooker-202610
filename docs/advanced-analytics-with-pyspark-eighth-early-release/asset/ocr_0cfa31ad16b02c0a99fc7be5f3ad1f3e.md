# Get indices of the vocabulary terms from our topics

Let us now generate the mappings from our topic indices to our vocabulary!

```
```python
for i, topic in enumerate(topics, start=1):
    print(f"topic {i}: {topic}")
...
```
```

topic 1: ['islam', 'health', 'drug', 'empir', 'medicin',
'polici', 'modul', 'expans', 'care', 'physician']
topic 2: ['formula', 'group', 'algebra', 'gener', 'transform',
'set', 'ideal', 'matrix', 'ring', 'elem']
topic 3: ['triangl', 'plane', 'line', 'point', 'two', 'tangent',
'contain', 'complex', 'side', 'geometri']
topic 4: ['face', 'therapeut', 'framework', 'particl', 'interf',
'rigid', 'theori', 'diffeomorph', 'gc', 'anatom']
topic 5: ['comput', 'polynomi', 'pattern', 'internet', 'network',
'equat', 'variabl', 'method', 'oper', 'coefici']

The above result is not perfect but there are some patterns that can be noticed in the topics above. Topic 1 is primarily related to health. It also contains reference to Islam, and empire. Could it be because of them being referenced in medicinal history and vice-versa or something else? Topic 2 and 3 are related to mathematics with the latter inclined towards geometry. Topic 5 is a mix of computing and mathematics. Even if you hadn't read any of the documents, you can already guess with a reasonable accuracy about their categories. This is exciting!

We can now also check which topics our input documents are most closely related with. A single document can have multiple topic associations which are significant. For now, we will only look at the most strongly associated topics.

Let's run the LDA model's transform operation on our input dataframe.

```
```python
lda_df = model.transform(vectorized_df)
lda_df.select(fun.col('title'),
fun.col('topicDistribution')).show(2, vertical=True,
truncate=False)
...
--RECORD 0------------------------------------------------------
```
```