# 第6章。使用LDA和Spark NLP理解维基百科

## 早期发布读者须知

通过早期发布电子书，您可以获取图书最早期的形式——作者撰写过程中的原始、未经编辑内容——因此您可以在这些书籍正式发布前很久就利用这些技术。

这将是最终书籍的第6章。

近年来，非结构化文本数据量不断增长，获取相关和所需信息变得困难。语言技术提供了强大的方法，可用于挖掘文本数据并获取我们正在寻找的信息。在本章中，我们将使用PySpark和Spark NLP库来使用这种技术——主题建模。具体来说，我们将使用潜在狄利克雷分配（LDA）来理解一组维基百科文档数据集。

主题建模是自然语言处理中最常见的任务之一。它是一种用于数据建模的统计方法，有助于发现文档集合中存在的潜在主题。从数百万份文档中提取主题分布在很多方面都很有用，例如识别特定产品或所有产品的投诉原因，或者更经典的例子是识别新闻文章中的主题。主题建模最流行的算法是LDA。它是一种生成模型，假设文档由主题分布表示，而主题反过来由单词分布表示。PySpark MLlib提供了一个经过优化的LDA版本，专门设计用于在分布式环境中工作。我们将使用Spark NLP对数据进行预处理，并使用Spark MLlib的LDA从数据中提取主题，构建一个简单的主题建模管道。

在本章中，我们将着手完成一项基于潜在（隐藏）主题和关系来提炼人类知识的适度任务。我们将把LDA应用于包含维基百科文章的语料库。我们将首先了解LDA的基础知识，并介绍其在PySpark中的实现。然后我们将下载数据集，并通过安装Spark NLP来设置编程环境。接下来将是数据预处理。您将见证Spark NLP库现成方法的威力，这些方法使NLP任务变得容易得多。

然后，我们将使用TF-IDF（术语频率-逆文档频率）技术对文档中的术语进行评分。生成的输出将随后输入我们的LDA模型，这是下一步。最后，我们将查看模型分配给输入文档的主题。我们应该能够在无需阅读条目的情况下理解条目所属的类别。让我们从回顾LDA的基础知识开始。

## 潜在狄利克雷分配

潜在狄利克雷分配（LDA）的背后思想是，文档基于一组主题生成。在此过程中，我们假设每个文档在主题上是分布的，每个主题在一组术语上也是分布的。每个文档和每个单词都是从这些分布中采样生成的。LDA学习者反向工作，试图识别观察结果概率最高的分布。

它试图将语料库提炼为一组相关主题。每个主题捕捉数据中的一种变化线索，通常对应于语料库讨论的某个想法。一个文档可以属于多个主题。您可以将LDA视为一种提供软聚类文档的方法。无需深入数学细节，LDA主题模型描述了两个主要属性：在采样特定文档时选择某个主题的概率，以及在选择主题时选择特定术语的概率。例如，LDA可能会发现一个与术语“Asimov”和“robot”以及文档“foundation series”和“science fiction”有强关联的主题。通过仅选择最重要的概念，LDA可以丢弃一些无关噪声，并合并共现的线索来得出数据的更简单表示。

我们可以在各种任务中运用这项技术。例如，在提供输入条目时，它可以帮助我们推荐相似的维基百科条目。通过封装语料库中的变化模式，它可以基于比单纯计算词语出现次数和共现次数更深层的理解来进行评分。这就是我们将在本章末尾要做的事情。接下来，让我们看看PySpark的LDA实现。

## PySpark中的LDA

PySpark MLlib将LDA实现作为其聚类算法之一。以下是一些示例代码：

```
from pyspark.ml.linalg import Vectors
from pyspark.ml.clustering import LDA
df = spark.createDataFrame([[1, Vectors.dense([0.0, 1.0])],
[2, Vectors.dense([2.0, 3.0])]],
["id", "features"])
lda = LDA(k=2, seed=1)
lda.setMaxIter(10)
model = lda.fit(df)
model.vocabSize()
2
model.describeTopics().show()
```

|主题|术语索引|术语权重|
|---|---|---|
|0|[0, 1]|[0.53331100994293...|
|1|[1, 0]|[0.50230220117597...|

我们将主题数（k）设置为2，并将LDA应用于我们的数据框。该数据框描述了我们主题中每个术语关联的概率权重

我们将在将LDA应用于维基百科数据集时探索PySpark的LDA实现及相关参数。在此之前，我们需要下载相关数据集。这就是我们接下来要做的事情。

## 获取数据

维基百科提供其所有文章的数据转储。完整转储位于一个大的XML文件中。这些文件可以下载，然后放置在云存储解决方案（S3、GCS）或HDFS中。例如：

```
$ curl -s -L https://dumps.wikimedia.org/enwiki/latest/\
$ enwiki-latest-pages-articles-multistream.xml.bz2 \
$   | bzip2 -cd \
$   | hadoop fs -put - wikidump.xml
```

这可能需要一些时间。

要处理如此大量的数据，最合理的做法是使用几个节点的集群。要在本地机器上运行本章代码，更好的选择是使用维基百科的导出页面生成较小的转储。尝试获取多个类别中的所有页面，这些类别拥有许多页面但很少子类别，例如Megafauna、Health和Geometry。为了让以下代码正常工作，请将转储下载到ch06-LDA/目录并将其重命名为wikidump.xml。

我们需要将维基百科XML转储转换为可以在PySpark中轻松处理的格式。在本地机器上工作时，我们可以使用方便的WikiExtractor工具。它从维基百科数据库转储（如我们拥有的）中提取和清理文本。

使用pip安装它。

```
$ pip3 install wikiextractor
```

然后只需在包含下载文件的目录中运行以下命令即可。

```
$ wikiextractor wikidump.xml
```

输出存储在给定目录中的单个或几个大小相似的文件中。每个文件将包含多个文档，格式如下。

```
$ tree wikidump
...
wikidump
└── AA
    └── wiki_00
...
$ head -n 5 wikidump/AA/wiki_00
...
```

```
<doc id="18831" url="?curid=18831" title="Mathematics">
Mathematics
```

数学（源自希腊语：）包括研究数字（算术和数论）、公式及相关结构（代数）、形状和空间（几何）以及数量及其变化（微积分和分析）等主题。关于其确切范围或认识论地位，没有普遍共识。
大部分数学活动包括发现和证明（通过纯推理）抽象对象的属性。这些对象要么源自自然（如自然数或“一条直线”），要么（在现代数学中）是由其基本属性定义的抽象实体，称为公理。证明包括将一些演绎规则应用于已知结果（包括先前证明的定理、公理以及在源自自然的情况下，一些被视为所考虑理论真实起点的基本属性）的连续过程。证明的结果称为“定理”。与物理定律相反，定理的有效性（其真理性）不依赖于任何实验，而依赖于其推理的正确性（尽管实验通常对发现新的有趣定理很有用）。

接下来，在开始处理数据之前，让我们先熟悉一下Spark NLP库。





## Spark NLP

Spark NLP库最初由John Snow Labs于2017年初设计，作为原生于Spark的标注库，以充分利用Spark SQL和MLlib模块。其灵感来自尝试使用Spark分发其他NLP库，而这些库通常不是以并发或分布式计算为设计目标实现的。

Spark NLP与任何其他标注库具有相同的概念，但在存储标注的方式上有所不同。大多数标注库将标注存储在文档对象中，但Spark NLP为不同类型的标注创建列。标注器作为Transformers、Estimators和Models实现。我们将在下一节应用它们到数据集进行预处理时探讨这些内容。在此之前，让我们下载并设置系统上的Spark NLP。

## FUNCTIONALITY AND ANNOTATION NLP LIBRARIES
NLP库大致可分为两种类型：功能性库和标注库。

### Functionality Libraries
### 功能性库

这些库是为特定的NLP任务和技术构建的。这些库提供的方法通常是自包含的。这意味着，假设在调用实体识别或词性（POS）标注方法时，仍需执行分词等基本任务。这使得它们更易于使用，但代价是性能，因为缺乏连贯的设计。

### Annotation Libraries
### 标注库

标注库是所有功能围绕文档-标注模型构建的库。它们围绕三个主要概念设计：文档、标注和标注器。

#### Document
#### 文档

它是我们想要分析或处理的文本的表示。

#### Annotation
#### 标注

它是我们NLP函数输出的表示。

#### Annotator
#### 标注器

它是包含使用NLP函数逻辑的对象。

随着我们在后续章节中使用Spark NLP，上述概念将变得更加清晰。

### Setting up your environment
### 设置环境

通过pip安装Spark NLP。

```
`pip3 install spark-nlp==3.2.3`
```

启动PySpark shell。

```
pyspark
```

让我们在PySpark shell中导入Spark NLP

```
import pyspark
from pyspark import SparkConf
from pyspark.sql import SparkSession
from pyspark.sql import functions as fun

packages = ','.join([
    "JohnSnowLabs:spark-nlp:1.6.3",
])

spark_conf = SparkConf()
spark_conf = spark_conf.setAppName('spark-nlp-book-p1c1')
spark_conf = spark_conf.setAppName('master[*]')
spark_conf = spark_conf.set("spark.jars.packages", packages)
spark = SparkSession.builder.config(conf=spark_conf).getOrCreate()
```

现在，您可以导入我们将使用的相关Spark NLP模块。

```
from sparknlp.base import DocumentAssembler, Finisher
from sparknlp.annotator import Lemmatizer, Stemmer, Tokenizer,
Normalizer, StopWordsCleaner
from sparknlp.pretrained import PretrainedPipeline
```

既然我们已设置好编程环境，让我们开始处理数据集。我们将首先将数据解析为PySpark DataFrame。

### Parsing the Data
### 解析数据

wikiextractor的输出可根据输入转储的大小创建多个目录。我们希望将所有数据作为单个DataFrame导入。让我们首先指定输入目录。

```
data_source = 'wikidump/*/*'
```

我们使用通过sparkContext可访问的wholeTextFiles方法导入数据。该方法将数据读入RDD。我们将其转换为DataFrame，因为这是我们想要的。

```
raw_data = spark.sparkContext.wholeTextFiles(data_source).toDF()
raw_data.show(1, vertical=True)
...
-RECORD 0-------------------------------
_1 | file:/home/analyt...
_2 | <doc id="18831" u...
```

生成的DataFrame将包含两列。记录数将对应所读取的文件数。第一列包含文件路径，第二列包含相应的文本内容。文本包含多个条目，但我们希望每一行对应一个条目。根据之前看到的条目结构，我们可以使用PySpark的split和 explode实用程序来分隔条目。

```
df = df.withColumn('content',
fun.explode(fun.split(fun.col("_2"), "</doc>")))
df = df.drop(fun.col('_2')).drop(fun.col('_1'))

df.show(4, vertical=True)
...
-RECORD 0-------------------------------
content | <doc id="18831" u...
-RECORD 1-------------------------------
content |
<doc id="5627588...
-RECORD 2-------------------------------
content |
<doc id="3354393...
-RECORD 3-------------------------------
content |
<doc id="5999808...
only showing top 4 rows
```

split()函数用于根据提供模式的匹配将DataFrame字符串列拆分为数组。在上述代码中，我们根据`</doc>`字符串将合并的文档XML字符串拆分为数组。这实际上给了我们一个包含多个文档的数组。然后，我们使用`explode()`为split函数返回的数组中的每个元素创建新行。这将为每个文档创建相应的行。

浏览我们之前操作中在content列中获得的结构。

```
df.show(1, truncate=False, vertical=True)
...
-RECORD 0
```

```
content | <doc id="18831" url="?curid=18831"
title="Mathematics">
Mathematics
Mathematics (from Greek: ) includes the study of such topics as
numbers (arithmetic and number theory), formulas and related
structures (algebra) shapes and spaces in which they are
contained (geometry), and quantities and ...
```

我们可以通过提取条目的标题来进一步拆分content列。

```
df = df.withColumn('title', fun.split(fun.col('content'),
'\n').getItem(2)) \
.withColumn('content', fun.split(fun.col('content'),
'\n').getItem(4))
df.show(4, vertical=True)
...
```

```
-RECORD 0------------------------ content | In mathematics, a... title | Tertiary
ideal -RECORD 1------------------------ content | In algebra, a bin... title |
Binomial (polynom... -RECORD 2------------------------ content | Algebra
(from ) i... title | Algebra -RECORD 3------------------------ content | In set
theory, th... title | Kernel (set theory) only showing top 4 rows ...
```

既然我们已经解析了数据集，让我们继续使用Spark NLP进行预处理。

### Preparing the data using Spark NLP
### 使用Spark NLP准备数据

我们之前提到，像Spark NLP这样的基于文档-标注器模型的库具有文档的概念。PySpark中原生不存在这样的概念。因此，Spark NLP的核心设计原则之一是与MLLib具有强互操作性。这是通过提供与DataFrame兼容的转换器来实现的，这些转换器将文本列转换为文档，并将标注转换为PySpark数据类型。

我们首先使用DocumentAssembler创建文档列。

```
```python
document_assembler = DocumentAssembler() \\
.setInputCol("content") \\
.setOutputCol("document") \\
.setCleanupMode("shrink")

document_assembler.transform(df).select('document').limit(1).collect()
...
Row(document=[Row(annotatorType='document', begin=0, end=289,
result='The kernel of a homomorphism is reduced to 0 (or 1) if
and only if the homomorphism is injective, that is if the inverse
image of every element consists of a single element. This means
that the kernel can be viewed as a measure of the degree to which
the homomorphism fails to be injective.', metadata={'sentence':
'0'}, embeddings=[]]))
```
```

> > 我们本可以在解析部分利用Spark NLP的DocumentNormalizer(https://nlp.johnsnowlabs.com/api/com/johnsnowlabs/nlp/annotators/DocumentNormalizer)标注器。

我们可以像上面那样直接转换输入dataframe。但是，我们将改为将DocumentAssembler和其他所需的标注器作为ML管道的一部分使用。

我们将在预处理管道中使用以下标注器：
- Tokenizer
- Normalizer
- StopWordsCleaner
- Stemmer

让我们从`Tokenizer`开始。

```
```scala
#### Split sentence to tokens (array)
tokenizer = Tokenizer() \\
    .setInputCols(["document"]) \\
    .setOutputCol("token")
```
```

分词器是基本的标注器。几乎所有基于文本的数据处理都以某种形式的分词开始。它是将原始文本分解为小块的过程。标记可以是单词、字符或子词（n-grams）。大多数经典NLP算法期望标记作为基本输入。许多深度学习算法正在开发中，以字符作为基本输入。大多数NLP应用程序仍然使用分词。

接下来是`Normalizer`。

```
```scala
#### clean unwanted characters and garbage
normalizer = Normalizer() \\
    .setInputCols(["token"]) \\
    .setOutputCol("normalized") \\
    .setLowercase(True)
```
```

Normalizer会清理上一步的标记，并从文本中删除所有不需要的字符。

接下来是`StopWordsCleaner`。

```
```scala
#### remove stopwords
stopwords_cleaner = StopWordsCleaner() \\
    .setInputCols("normalized") \\
    .setOutputCol("cleanTokens") \\
    .setCaseSensitive(False)
```
```

这个标注器从文本中删除“停用词”。停用词如the、is、at等词非常常见，可以在不显著


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

#### train the model
cv_model = cv.fit(tokens_df)

#### transform the data. Output column name will be raw_features.
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





```
vectorized_df = vectorized_df.drop(fun.col('raw_features'))
vectorized_df.show(6)
...
```

| 标题 | 词条 | 特征 |
|---|---|---|
| 三元理想 | [mathemat, tertia... | (2451, [1, 6, 43, 56,... |
| 二项式 (多项式... | [algebra, binomi,... | (2451, [0, 10, 14, 34... |
| 代数 | [algebra, on, bro... | (2451, [0, 1, 5, 6, 15... |
| 内核 (集合论) | [set, theori, ker... | (2451, [2, 3, 13, 19,... |
| 广义算术... | [mathemat, gener,... | (2451, [1, 2, 6, 45, 4... |

在完成所有预处理和特征工程后，我们现在可以创建 LDA 模型了。我们将在下一节中完成此操作。

## 创建我们的 LDA 模型

我们之前提到过，LDA 将语料库提炼为一组相关主题。我们将在本节后面部分查看此类主题的示例。在此之前，我们需要决定 LDA 模型所需的 2 个必需超参数。它们分别是主题数量（称为 k）和迭代次数。

选择 k 有多种方法。用于此目的的两个流行指标是困惑度和主题连贯性。前者由 PySpark 的实现提供。基本思路是尝试找出这些指标的改进开始变得不显著的 k 值。如果你熟悉用于查找 K-means 聚类数量的肘部法则，这与此类似。根据语料库的大小，这可能是一个资源密集且耗时的过程，因为你需要为多个 k 值构建模型。另一种替代方案是尝试创建手头数据集的代表性样本，并使用它来确定 k。阅读并尝试此方法被留作练习。

由于你当前可能在本地工作，我们现在将分配合理的值（k 为 5，max_iter 为 50）。

### 让我们创建 LDA 模型。

```
from pyspark.ml.clustering import LDA

num_topics = 5
max_iter = 50

lda = LDA(k=num_topics, maxIter=max_iter)
model = lda.fit(vectorized_df)

lp = model.logPerplexity(vectorized_df)
print("The upper bound on perplexity: " + str(lp))
...
The upper bound on perplexity: 6.768323190833805
```

困惑度是衡量模型预测样本效果的指标。低困惑度表明概率分布在预测样本方面效果良好。在比较不同模型时，选择困惑度值较低的模型。

现在我们已经创建了模型，希望将主题输出为人类可读的形式。我们将获取预处理步骤生成的词汇表，从 LDA 模型中获取主题，并将两者映射。

```
vocab = cv_model.vocabulary

raw_topics = model.describeTopics().collect()

topic_inds = [ind.termIndices for ind in raw_topics]

topics = []
for topic in topic_inds:
    _topic = []
    for ind in topic:
        _topic.append(vocab[ind])
    topics.append(_topic)
```

- 创建对我们词汇表的引用。
- 使用 describeTopics() 获取 LDA 模型生成的主题，并将其加载到 Python 列表中。

### 从主题中获取词汇表术语的索引

让我们现在从主题索引生成到我们词汇表的映射！

```
for i, topic in enumerate(topics, start=1):
    print(f"topic {i}: {topic}")
...
```

主题 1: ['islam', 'health', 'drug', 'empir', 'medicin',
'polici', 'modul', 'expans', 'care', 'physician']
主题 2: ['formula', 'group', 'algebra', 'gener', 'transform',
'set', 'ideal', 'matrix', 'ring', 'elem']
主题 3: ['triangl', 'plane', 'line', 'point', 'two', 'tangent',
'contain', 'complex', 'side', 'geometri']
主题 4: ['face', 'therapeut', 'framework', 'particl', 'interf',
'rigid', 'theori', 'diffeomorph', 'gc', 'anatom']
主题 5: ['comput', 'polynomi', 'pattern', 'internet', 'network',
'equat', 'variabl', 'method', 'oper', 'coefici']

上述结果并不完美，但上述主题中可以注意到一些模式。主题 1 主要与健康相关。它还包含对伊斯兰和帝国的引用。这可能是因为它们在医学史中被互相引用，或者是其他原因？主题 2 和 3 与数学相关，后者更偏向几何。主题 5 是计算和数学的混合。即使你没有阅读过任何文档，也可以相当准确地猜测它们的类别。这很令人兴奋！

我们现在还可以检查输入文档与哪些主题最密切相关。单个文档可以有多个显著的主题关联。目前，我们只查看最强相关的主题。

让我们在输入数据框上运行 LDA 模型的 transform 操作。

```
lda_df = model.transform(vectorized_df)
lda_df.select(fun.col('title'),
fun.col('topicDistribution')).show(2, vertical=True,
truncate=False)
...
--记录 0------------------------------------------------------
```

| 标题 | 主题分布 |
|---|---|
| 三元理想 | [5.673953573608612E-4,0.6381457706306267,0.29869040292311466,5.515512094400066E-4,0.06204487987945782] |
| 二项式 (多项式) | [0.0019374384060205127,0.99094419394645,0.00229391661948347,0.0018833398556289728,0.0029411111724170704] |

仅显示前 2 行

如上所示，每个文档都有一个关联的主题概率分布。要获取每个文档的关联主题，我们需要找出概率得分最高的主题索引。然后可以将其映射到我们之前获得的主题。

我们将编写一个 PySpark UDF 来查找每条记录的最高主题概率得分。

```
max_index = fun.udf(lambda x: x.tolist().index(max(x)) + 1,
IntegerType())
lda_df = lda_df.withColumn('topic_index',
max_index(fun.col('topicDistribution')))

lda_df.select('title', 'topic_index').show(10, truncate=False)
...
```

| 标题 | 主题索引 |
|---|---|
| 三元理想 | 2 |
| 二项式 (多项式) | 2 |
| 代数 | 2 |
| 内核 (集合论) | 2 |
| 广义等差数列 | 2 |
| Schur 代数 | 2 |
| 代数纲要 | 2 |
| 递归关系 | 5 |
| 有限差方程 | 5 |
| 多项式运算 | 2 |

仅显示前 11 行

主题 2（如果你还记得的话）与数学相关。上述输出符合我们的预期。你可以浏览更多数据集以查看其表现。

## 后续方向

在本章中，我们对维基百科语料库进行了 LDA。在此过程中，我们还了解了使用出色的 Spark NLP 库和 TF-IDF 技术进行文本预处理。你可以通过更好的预处理和超参数调优来改进模型，从在此基础上进一步构建。此外，你甚至可以尝试根据文档相似度，基于用户输入推荐类似条目。这种相似度可通过使用从 LDA 获得的概率分布向量来获取。

此外，还有多种其他方法用于理解大型文本语料库。例如，一种称为潜在语义分析（LSA）的技术在类似应用中很有用，并且在之前的版本中用于同一数据集。第 10 章中探讨的深度学习，也提供了使用 Topic2Vec 等新模型进行主题建模的途径。你可以探索它们。
