

改变文本的含义。当只想处理文本中语义上最重要的单词，并忽略那些很少具有语义相关性的单词（如冠词和介词）时，删除停用词非常有用。

```
stemmer = Stemmer() \
    .setInputCols(["cleanTokens"]) \
    .setOutputCol("stem")
```

Stemmer 的目的是从单词中提取词干，以获取单词的有意义部分。词干提取是将单词还原为其词根词干的过程，其目标也是提取单词的有意义部分。例如，“picking”、“picked” 和 “picks” 的词根均为 pick。

我们几乎完成了。在完成 NLP 管道之前，我们需要添加 Finisher。当将 DataFrame 中的每一行转换为文档时，Spark NLP 会添加其自身的结构。Finisher 至关重要，因为它帮助我们恢复预期的结构——令牌数组。

```
finisher = Finisher() \
    .setInputCols(["stem"]) \
    .setOutputCols(["tokens"]) \
    .setOutputAsArray(True) \
    .setCleanAnnotations(False)
```

现在所有必需的组件都已就位。让我们构建管道，以便每个阶段可以按顺序执行。

```
nlp_pipeline = Pipeline(
    stages=[document_assembler,
            tokenizer,
            normalizer,
            stopwords_cleaner,
            stemmer,
            finisher])
```

执行管道并转换 DataFrame。

```
nlp_model = nlp_pipeline.fit(df)
processed_df = nlp_model.transform(df)
processed_df.printSchema()
...
root
|-- content: string (nullable = true)
|-- title: string (nullable = true)
|-- document: array (nullable = true)
|    |-- element: struct (containsNull = true)
|    |    |-- annotatorType: string (nullable = true)
|    |    |-- begin: integer (nullable = false)
|    |    |-- end: integer (nullable = false)
|    |    |-- result: string (nullable = true)
|    |    |-- metadata: map (nullable = true)
|    |    |    |-- key: string
|    |    |    |-- value: string (valueContainsNull = true)
|    |-- embeddings: array (nullable = true)
|    |    |-- element: float (containsNull = false)
|-- token: array (nullable = true)
|    |-- element: struct (containsNull = true)
|    |    |-- annotatorType: string (nullable = true)
|    |    |-- begin: integer (nullable = false)
|    |    |-- end: integer (nullable = false)
|    |    |-- result: string (nullable = true)
|    |    |-- metadata: map (nullable = true)
|    |    |    |-- key: string
|    |    |    |-- value: string (valueContainsNull = true)
|    |-- embeddings: array (nullable = true)
|    |    |-- element: float (containsNull = false)
|-- normalized: array (nullable = true)
|    |-- element: struct (containsNull = true)
|    |    |-- annotatorType: string (nullable = true)
|    |    |-- begin: integer (nullable = false)
|    |    |-- end: integer (nullable = false)
|    |    |-- result: string (nullable = true)
|    |    |-- metadata: map (nullable = true)
|    |    |    |-- key: string
|    |    |    |-- value: string (valueContainsNull = true)
|    |-- embeddings: array (nullable = true)
|    |    |-- element: float (containsNull = false)
|-- cleanTokens: array (nullable = true)
|    |-- element: struct (containsNull = true)
|    |    |-- annotatorType: string (nullable = true)
|    |    |-- begin: integer (nullable = false)
|    |    |-- end: integer (nullable = false)
|    |    |-- result: string (nullable = true)
|    |    |-- metadata: map (nullable = true)
|    |    |    |-- key: string
|    |    |    |-- value: string (valueContainsNull = true)
|    |-- embeddings: array (nullable = true)
|    |    |-- element: float (containsNull = false)
|-- stem: array (nullable = true)
|    |-- element: struct (containsNull = true)
|    |    |-- annotatorType: string (nullable = true)
|    |    |-- begin: integer (nullable = false)
|    |    |-- end: integer (nullable = false)
|    |    |-- result: string (nullable = true)
|    |    |-- metadata: map (nullable = true)
|    |    |    |-- key: string
|    |    |    |-- value: string (valueContainsNull = true)
|    |-- embeddings: array (nullable = true)
|    |    |-- element: float (containsNull = false)
|-- tokens: array (nullable = true)
|    |-- element: string (containsNull = true)
```

## 训练管道并将管道应用于转换 DataFrame

NLP 管道创建了我们不需要的中间列。让我们删除冗余列。

```
tokens_df = processed_df.select('title','tokens') # .limit(10000)
tokens_df.show(2, vertical=True)
...
```

| 记录 | 标题 | 标记 |
|------|------|------|
| 0 | Tertiary ideal | [mathemat, tertia... |
| 1 | Binomial (polynom... | [algebra, binomi,... |

仅显示前 2 行

接下来，我们将了解 TF-IDF 的基础知识，并在构建 LDA 模型之前，在我们之前获得的预处理数据集 token_df 上实现它。

## TF-IDF

在应用 LDA 之前，我们需要将数据转换为数字表示。我们将使用术语频率 - 逆文档频率（TF-IDF）方法获得这种表示。大致而言，TF-IDF 用于确定给定文档中术语的重要性。以下是该公式的 Python 代码表示。我们实际上不会使用这段代码，因为 PySpark 提供了自己的实现。

```
import math
def term_doc_weight(term_frequency_in_doc, total_terms_in_doc,
                    term_freq_in_corpus, total_docs):
    tf = term_frequency_in_doc / total_terms_in_doc
    doc_freq = total_docs / term_freq_in_corpus
    idf = math.log(doc_freq)
    return tf * idf
```

TF-IDF 捕捉了关于术语与文档相关性的两个直觉。首先，我们预期一个术语在文档中出现的频率越高，它对该文档就越重要。其次，从全局角度看，并非所有术语都同等重要。在整个语料库中很少出现的单词，比在大多数文档中出现的单词更有意义，因此该指标使用了单词在完整语料库文档中出现次数的逆值。

语料库中单词的频率通常呈指数分布。常见单词的出现频率通常是不太常见单词的十倍，而后者反过来可能比罕见单词高出十倍或一百倍。如果基于原始逆文档频率来构建指标，将会赋予罕见单词巨大的权重，并在实际中忽略所有其他单词的影响。为了捕捉这种分布，该方案使用了逆文档频率的对数。这通过将频率之间的乘法差距转化为加法差距，缓和了文档频率的差异。

该模型基于几个假设。它将每个文档视为“词袋”，这意味着它不关注单词的顺序、句子结构或否定。由于每个术语只表示一次，模型在处理*多义性*（即同一词用于多种含义）方面存在困难。例如，模型无法区分“Radiohead is the best band ever”和“I broke a rubber band”中“band”的用法。如果这两个句子在语料库中都经常出现，它可能会将“Radiohead”与“rubber”联系起来。

现在让我们继续使用 PySpark 实现 TF-IDF。

### 计算 TF-IDF

首先，我们将使用 CountVectorizer 计算 TF（文档中每个术语的频率）。CountVectorizer (TF) 会跟踪正在创建的词汇表，以便我们可以将主题映射回对应的单词。TF（术语频率）会创建一个矩阵，统计词汇表中每个单词在每段文本中出现的次数。然后根据单词的频率为每个单词赋予权重。我们在拟合过程中推导数据的词汇表，并在转换步骤中获取计数。

```
from pyspark.ml.feature import CountVectorizer
cv = CountVectorizer(inputCol="tokens", outputCol="raw_features")

# train the model
cv_model = cv.fit(tokens_df)

# transform the data. Output column name will be raw_features.
vectorized_tokens = cv_model.transform(tokens_df)
```

然后，我们继续进行 IDF（术语出现的文档的逆频率），它会降低常见术语的权重。

```
from pyspark.ml.feature import IDF
idf = IDF(inputCol="raw_features", outputCol="features")

idf_model = idf.fit(vectorized_tokens)

vectorized_df = idf_model.transform(vectorized_tokens)
```

结果将如下所示。

