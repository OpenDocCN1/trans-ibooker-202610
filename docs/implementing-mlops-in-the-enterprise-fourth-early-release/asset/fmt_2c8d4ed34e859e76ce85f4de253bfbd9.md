
```
#!/usr/bin/env bash
source /home/codespace/venv/bin/activate
#append it to bash so every shell launches with it
echo 'source /home/codespace/venv/bin/activate' >> ~/.bashrc
```

One of the fantastic things about this workflow is that it makes it possible to run GPU containers and GPU projects. [Figure 9-19](#ch09.xhtml#Figure-10-14-mlops-tensorflow-gpu) shows the Tensorflow Docker container running inside GitHub codespaces using the GPU.

![](assets/10-14-mlops-tensorflow-gpu-light.png)

You can read more about [TensorFlow GPU documentation here.](https://www.tensorflow.org/install/docker). A few things to point out include the following commands:

* Run `docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu \ python -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"`
* Also interactively explore: `docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu`, then when inside run: `apt-get update && apt-get install pciutils` then `lspci | grep -i nvidia`
* To mount the code into your container: `docker run --gpus all -it --rm -v $(pwd):/tmp tensorflow/tensorflow:latest-gpu /bin/bash`. Then do `apt-get install -y git && cd /tmp`. Then all you need to do is run `make install`. Now you can verify you can train deep learning models by doing `python utils/quickstart_tf2.py`

# Bridging the Gap to Notebooks

(*guest author: andrew cui* from Linea)

Another strategy to ditch Notebooks in the MLOps cycle is to bridge the gap between the monolithic Juypter Notebooks and the cloud-native development environment. Instead of having the center of a workflow for data science be the Notebook, it should serve as a gateway to the rest of the MLOps development ecosystem.

This type of workflow is already illustrated in [Figure 9-15](#ch09.xhtml#Figure-10-8-mlops-v2) by the Notebook as a secondary tool. However, even using the notebook as a secondary tool in a cloud-native development environment brings with it many of the old challenges faced by the “lift and shift” paradigm of productionizing Notebooks in a Notebook-centric environment.

## Problems with Notebooks: Linea Can Help

There are many significant challenges brought about by using Notebooks even as a secondary tool in an MLOps-centric development environment when trying to take work from the Notebook to other MLOps-focused tools. Data science and engineering teams face many common problems including:

* *Dependency management and toolchain setup*: Many headaches come from having the development environment set up with the same dependencies and toolchains as the production environment. This process can cause issues when trying to promote components and pipelines to run in production, as they will fail due to a lack of dependencies or missing tools.
* *Management and organization of reusable code components*: A common theme among inefficient organizations with poor MLOps practices is the need for more ease of sharing code modules, leading to a proliferation of repeated code with very little code management. This process makes it much more difficult from an ops perspective to identify these reusable components so they can have good operational practices applied to them, including reviewing, testing, and deploying their code.
* *Recovering and cleaning code*: In many organizations, the engineers tasked with taking notebooks to production are given messy unreadable notebooks, further complicating the above issues. Figuring out which cells are not necessary to the primary data flow is complex, and finding code that may have been in a deleted partition or executed out of order is very difficult from just the notebook is nearly impossible.

## What Linea Does

[Linea](https://linea.ai/) is a tool that focuses on tackling these problems, positions itself between the Jupyter Notebook, and connects it to all the other essential MLOps tools in a cloud-based development environment. This step allows for ease of integrating Jupyter Notebooks into the cloud-based development environmen while avoiding many problems associated with using Jupyter Notebooks.

Figure [Figure 9-18](#ch09.xhtml#Figure-10.11-devops-in-the-loop) illustrates where these new tools live and where they can provide benefits.

Linea can help with many of the complicated and intractable tasks of automatically bridging this gap by capturing and understanding the execution of the Notebook. Linea uses a semantic understanding of what the user ran to offer features necessary for an Ops-focused organization and directly tackles the problems using notebooks as a secondary tool.

## Solution: Dependency Management

One of the essential functionalities Linea offers is automatically creating docker files that containerize functions run in the Notebook. The ability to containerize workloads is at the heart of good MLOps practice. It is critical for scaling the capabilities of a data organization and avoiding costly issues of mismatched dependencies or toolchain wrangling.

Linea can handle dependency resolution automatically, creating Dockerfiles that regulate the installation of the minimum dependencies and tools necessary to run its payload. This step is crucial since data scientists import a lot of dependencies in their notebooks, leading to a “kitchen sink” of dependencies with all the libraries and tools they’ve ever needed to use.

By identifying the minimum set of dependencies, Linea makes it easier to set up and deploy containerized workloads and improves the reliability of these workloads as well.

The following is an example of how easy and [`https://docs.lineapy.org/en/latest/guide/build\_pipelines/pipeline\_basics.xhtml?highlight=dockerfile#output-files`](https://docs.lineapy.org/en/latest/guide/build_pipelines/pipeline_basics.xhtml?highlight=dockerfile#output-files) \[automatic this can be\].

## Solution: Identifying and using reusable components

Another critical pillar of good MLOps practice is the ability to treat code as code and use the appropriate tools to manage it. As discussed earlier, the GitHub Ecosystem is also critical to managing code. Still, a problem arises in identifying the pieces of code from a notebook taken to the GitHub ecosystem.

Fortunately, Linea can capture the semantics of what the user runs in the notebook environment and suggest high-value reusable components shared and used by many to be added and maintained in the GitHub Ecosystem. Examples of standard reusable components include an extract transform load (ETL) component to transform critical and commonly used datasets to a format used by the rest of the organization’s libraries or perhaps a model training loop that gets the credentials necessary to access GPU resources in the organization.

Once these reusable components are under the management of the Github Ecosystem, they can be versioned, edited, and developed with good MLOps practices in mind, totally avoiding the pitfalls of managing critical code in the Jupyter environment.

## Solution: Cleaning Code From Notebooks

Good clean code is a hallmark of good DevOps practices, and the same applies to MLOps. Clean code, which has been broken down nicely into functions with unnecessary code removed, is critical for understanding and taking work from notebooks to other MLOps tools which work with a production-first mindset.
