
## Production-First Container Mindset

3. Containers are a production-first mindset in that your customer is the production deployment, and you build for it initially.

4. [Distroless containers](https://github.com/GoogleContainerTools/distroless) allow minimal container size, as tiny as 20MB for Microservices with [compiled languages](https://github.com/nogibjj/rust-mlops-template/tree/main/distroless-web).

In practice, this means an MLOps workflow starts with the following steps in which containers play a critical role:

1. Provision a cloud-based IDE with a containerized environment almost identical to production (or very close).

2. Set up the DevOps pipeline, i.e., `Makefile` (or equivalent), and get the build system working for each step: `install`, `test`, `lint`, `format`, `deploy`, etc. You can see these exact steps in this [GitHub Actions `cicd.yml` file.](https://github.com/nogibjj/mlops-template/blob/GPU/.github/workflows/cicd.yml)

```
name: CI
on:
  push:
    branches: [ "GPU" ]
  pull_request:
    branches: [ "GPU" ]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: install packages
        run: make install
      - name: lint
        run: make lint
      - name: test
        run: make test
      - name: format
        run: make format
      - name: deploy
        run: make deploy
```

These first two steps correspond to the actual [`Makefile`](https://github.com/nogibjj/mlops-template/blob/GPU/Makefile) and allow for an easy link from local testing to testing on the build system.

* Enable AI pair programming tools, like [GitHub Copilot](https://github.com/features/copilot) with Codespaces or Cloud9 with [Code Whisperer](https://aws.amazon.com/codewhisperer/). These steps embed into a reproducible `devcontainer.json` configuration, as shown in the following section of the file. Each extension is prebuilt and preloaded in the environment:

```
 // Add the IDs of extensions you want installed when the container is created:
	 "extensions": [
	 "GitHub.vscode-pull-request-github",
	 "GitHub.copilot-nightly",
	 "GitHub.copilot-labs",
	 "ms-azuretools.vscode-docker",
	 "ms-toolsai.jupyter",
	 "ms-toolsai.jupyter-keymap",
	 "ms-toolsai.jupyter-renderers",
	 "ms-python.vscode-pylance",
	 "ms-python.python",
	 "ms-vscode.makefile-tools"
	 ]
```

* Write library code (which may use a pre-trained model, i.e., Hugging Face) or use a model in a model registry. You can see a module [library here](https://github.com/nogibjj/assimilate-openai/tree/main/oalib).

* Turn library code into a command-line tool first to prototype it. You can see an example of a [cli tool here](https://github.com/nogibjj/assimilate-openai/blob/main/questionAnswerCLI.py).

* Then invoke library code through Jupyter, FastAPI, [Streamlit](https://streamlit.io/), [Gradio](https://gradio.app/), etc., i.e. clients of the code.

In practice, what this means is that with Python functions, you can build once, i.e., your library code, then “use many” and “deploy many,” as shown in [Figure 9-4](#ch09.xhtml#Figure-10-16-functions-python). This fact is why functional programming is critical to building a DevOps foundational layer for projects doing machine learning. If you first create a library that conforms to DevOps best practices, then adding this onto an MLOps workflow in which a Jupyter Notebook may or may exist is straightforward.

![](assets/10-16-functions-python.png)

# Building Solutions with Pre-Trained Models

One unique aspect of MLOps is that you don’t always need to build the model yourself to embrace the methodology surrounding MLOPs. Several important trends support this movement:

* Carbon footprint of machine learning

* Microeconomic principles of economies of scale.

* Emerging technology solutions and platforms. At the forefront of this movement are two companies: [OpenAI](https://openai.com) and [Hugging Face](https://huggingface.co).

Later in the chapter, this container-first mindset manifests itself in building solutions with GitHub Codespaces.

## Carbon Footprint of Machine Learning

A significant challenge in modern machine learning is managing the carbon footprint of machine learning. A recent paper [The Carbon Footprint of Machine Learning Training Will Plateau, Then Shrink](https://arxiv.org/pdf/2204.05149.pdf) enumerates four practices that reduce energy:

1. *Model*: Selecting efficient models reduces computation by 5-10 times.

2. *Compute*: More specialized chips like [TPUs](https://cloud.google.com/tpu/docs/tpus) or modern GPUs are more efficient at reducing energy 1.4-2 times than other forms of a compute like a CPU.

3. *Cloud*: Efficient cloud data center energy can reduce carbon usage by 1.4-2 times.

4. *Regional specificity*: Cloud computing users can select data center regions with the highest percentage of clean energy.

While these factors are appealing and actionable, there is yet another component to reducing the carbon footprint of machine learning. Using pre-trained models eliminates the need to train new models or significantly reduces the training time through [fine-tuning](https://huggingface.co/docs/transformers/training).

###### Note

*Fine-tuning* is a technique that allows a practitioner to leverage highly complex machine-learning models with billions of parameters and create specialized versions of these models through minimal data and training.

## Economies of scale of Pre-Trained Models

Several microeconomic trends support using pre-trained models. Let’s start with the comparative advantage example. Large organizations can access thousands of expensive GPUs that train models with billions of parameters. Additionally, world-class machine learning engineers help architect models on tasks for computer vision, natural language processing, and more. Organizations that produce these sophisticated pre-trained models have a comparative advantage over smaller organizations or individuals.

As a result, there is much to be said for using pre-trained models either as they come or fine-tuning them. Being thoughtful about comparative advantage is a way to leverage the power of experts, get better business outcomes, and reduce carbon footprint.

# Emerging Technologies and Platforms for Pre-trained Models

The early part of 2020 saw the emergence of the pre-trained model revolution. Companies like [Hugging Face](https://huggingface.co/), [Stability AI](https://stability.ai/faq), and [Open AI](https://openai.com/) have pushed the envelope on a new form of machine learning in the form of pre-trained large models.

## LLM (Large Language Models)

Stanford’s Institute for Human-Centered Artificial Intelligence, in a recent article on large language models, [argues that LLMs will transform society](https://hai.stanford.edu/news/how-large-language-models-will-transform-science-society-and-ai). To show this idea in action, I used the Hugging Face interface and their BigScience, or Bloom Model, to help write a section of this book seen below, as shown in [Figure 9-5](#ch09.xhtml#Figure-10-4-bloom-llm). Notice the first sentence, *“Large language models can completely transform how we see the world.”* prompted the second sentence, *“They can be used to understand the meaning of a sentence, to generate new sentences, and to answer questions”.*

![](assets/10-4-bloom-llm.png)

One of the more famous examples of an LLM is GPT-3, [released in July 2020 by OpenAI](https://openai.com/blog/gpt-3-apps/). One of the more powerful new technologies is, [ChatGPT](https://chat.openai.com/chat) which can do things like write code on the fly and answer general search queries like “what is the life expectancy around the world for women?”.

###### Note
