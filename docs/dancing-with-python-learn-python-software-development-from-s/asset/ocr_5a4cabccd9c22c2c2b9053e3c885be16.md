# 12.3.3 NLP with spaCy

To go beyond characters, words, numbers, whitespace, and patterns, we need more sophisticated tools trained on many documents to recognize parts of speech, word roots, phrases, structure, names of people and locations, and other language features. One such Python tool is spaCy, which you install from the command line as the **spacy** package.

```
```sh
pip install spacy
python -m spacy download en_core_web_sm
```
```

This section introduces the basic functionality of **spacy**, but I encourage you to read the tutorial and documentation to see more examples and the breadth of functionality. [SCY]

# Tokenization

We begin by importing **spacy** and loading the “small English database” into the nlp engine.

```
```python
import spacy

nlp = spacy.load("en_core_web_sm")
```
```

We now process our document, which is the string we have been using for the previous examples. In practice, the document might be a paragraph, an article, a book, a web page, a collection of social media entries, a customer’s purchase history, chatbot questions, or any *corpus* of text we wish to analyze. The result is a sequence of *tokens*:

```
```python
doc = nlp("I'm making more fried green tomatoes.")

print(" | ".join([token.text for token in doc]))
```
```

```
```text
I | 'm | making | more | fried | green | tomatoes | .
```
```

A token is a word or word fragment, and *tokenization* is the process of creating tokens.

# Lemmas and parts of speech

In addition to its contained characters, each token in the document includes language properties. Here is a formatted table of the document’s tokens and some of their properties:

```
```python
for token in doc:
    print(f"{token.text:9} {token.lemma_:8} "
          f"{token.pos_:8} {token.dep_:8}")
```
```

| Token | Lemma | POS | Dep |
|---|---|---|---|
| I | I | PRON | nsubj |
| 'm | be | AUX | aux |