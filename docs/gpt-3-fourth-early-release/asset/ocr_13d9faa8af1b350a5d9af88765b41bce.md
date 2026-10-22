Even if the gap between English (Nr 1, 93% of dataset) and German (Nr 2, 1% of dataset) is dramatic, it’s already sufficient to create perfect text in German with style transfer and other tasks. The same goes for other languages on the list.

Since GPT-3 is pre-trained on an extensive and diverse corpus of text, it can successfully perform a surprising number of NLP tasks without users providing any additional example data. The model training stopped in 2019, so the database does not contain omnipresent topics like COVID-19, Biden as the president of the USA etc.

# Transformer models

In order to understand how GPT-3 was built, we need to dive into its underlying architecture: transformers. Neural networks form the core of deep learning. Their name and structure are inspired by the human brain, mimicking the way that biological neurons signal to one another. A neural network is a network or circuit of neurons working in tandem. We’ll look at two kinds of neural networks here: sequence-to-sequence models and transformer models.

# Sequence-to-sequence models

The paper “Attention Is All You Need”(Vaswani, Ashish & Shazeer, Noam & Parmar, Niki & Uszkoreit, Jakob & Jones, Llion & Gomez, Aidan & Kaiser, Lukasz & Polosukhin, Illia (2017)) by Google Research and Toronto University team introduces the idea of a transformer model: