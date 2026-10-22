

The connection to OpenAI API is now established in the form of a “service” object. We will now go ahead and read the training prompt from the prompts folder:

```
// Reading the training prompt from the prompts folder
File file = new File("D:\\GPT-3 Book\\Programming with GPT-3\\GPT-3 Java\\example\\src\\main\\java\\example\\prompts\\summarize_for_a_2nd_grader.txt");

Scanner sc = new Scanner(file);

// we just need to use \\Z as delimiter
sc.useDelimiter("\\Z");
// pp is the string consisting of the training prompt
String pp = sc.next();
```

Once you have the training prompt in place, you can create a completion request with the following configuration for the API parameters:

```
Maximum tokens=100, Execution Engine="Davinci",
Temperature=0.5, Top-p=1, Frequency Penalty = 0.2,
Stop Sequence = ["\n\n"]

// Creating a list of strings to used as stop sequence
List<String> li = new ArrayList<String>();
li.add("\n\n''");
// Creating a completion request with the API parameters
CompletionRequest completionRequest =
CompletionRequest.builder().prompt(pp).maxTokens(100).temperature(0.5).topP(1.0).frequencyPenalty(0.2).stop(li).echo(true).build();
// Using the service object to fetch the completion response
service.createCompletion("davinci",completionRequest).getChoices().forEach(System.out::println);
```

We can then save the code file as ‘text_summarization.java’ and run it from the terminal to generate the output. You can use the following command to run the file from your root folder:

```
(base) PS D:\GPT-3 Java> ./gradlew example:run
```

Once we executed the file, our console printed the following output:

```
--------------------
Olive oil is a liquid fat that comes from olives. Olives grow on a tree called an olive tree. The olive tree is the most common tree in the Mediterranean. People use the oil to cook with, to put on their
salads, and as a fuel for lamps.
---------------------
```

For more examples of how you can use Java programming to interact with GPT-3, visit the following github repository.

## The GPT-3 Sandbox

By now, you should be familiar with the basic components required to build a GPT-3 powered application. In this section we will walk you through the GPT-3 Sandbox, an open-source tool that will help you to turn your ideas into reality with just a few lines of python code.

The goal of our sandbox is to empower the readers of this book to create cool web applications agnostic of their technical backgrounds. The sandbox is designed to be low-code to make it user-friendly for people with little to no technical background.

It is built on top of the Streamlit framework enabling users to quickly create & deploy GPT-3 based web applications and showcase their ideas to a wider audience.

As part of the book resources, we have also created a video series with a step-by-step walkthrough on creating and deploying your GPT-3 application. Please follow the video series along with this tutorial to better understand the sandbox ecosystem.

In this tutorial we will show you how to use the GPT-3 sandbox and customize it for your specific application. For this tutorial we will be using VSCode as the IDE, but feel free to use any IDE of your choice.

Prerequisites

- 1. Install VSCode / Any other IDE
- 2. Python 3.7 and above

To use the GPT-3 sandbox, clone the code from the following github repository to your favorite development environment (we will use vscode for this example). You can do it by simply opening a new terminal in your IDE, and using the following command:

```
git clone https://github.com/Shubhamsaboo/kairos_gpt3
```

After cloning the repository just make sure that you are using Python version 3.7+ installed by running the following command:

```
python --version
```

The code structure in your IDE should now look as follows:

![](img/495f94a597dee9cc5b6b1e9654aa4339_84_0.png)

Figure 3-1. Sandbox File Directory Structure

Everything you need to create and deploy a web application is already present in the code. You just need to tweak a few files to customize the functioning of the sandbox for your specific use case.

- 1. After cloning the git repository, the next step is to create a python virtual environment by following the instructions mentioned in this document.

After creating & activating the virtual environment, now its time to install the required dependencies.

At this point we have created and activated a python virtual environment named “env”. Now from here you go to the email_generation folder. To make sure you are in the right space, your path should look like this:

```
(env) kairos_gpt3\GPT-3 Sandbox\email_generation>
```

And from there run the following command:

```
(env) kairos_gpt3\GPT-3 Sandbox\email_generation> pip install
-r requirements.txt
```

- 2. After installing the dependencies you can start customizing the sandbox code. The first file that you need to look at is training_data.py.

In that file you can replace the default prompt with the training prompt specific to your use-case. To play and experiment with different training prompts for your application, you can use the GPT-3 playground as we discussed in chapter 2. Refer to the following video for a visual walkthrough of customizing the sandbox.

- 3. Once you are done with replacing the training prompt based on your use-case you can then tweak the API parameters (Maximum tokens, Execution Engine, Temperature, Top-p, Frequency Penalty, Stop Sequence) as per your requirements in the training_service.py file.

- 4. That’s it! Your GPT-3 based web application is now ready and can be run locally using the following command

```
(env) kairos_gpt3\GPT-3 Sandbox\email_generation> streamlit run gpt_app.py
```

5. Now after checking the application sanity locally, its time for you to deploy the application to the internet using Streamlit sharing (refer to the following video for the deployment walkthrough) to showcase it to a wider audience.

## NOTE

This application follows a simple workflow, where the training prompt receives single input from the UI and comes up with the response. If your application requires complex workflow, where the training prompt takes in multiple inputs to come up with the output. You can go ahead and customize the UI elements by going through the following scripts (app1.py, app2.py, gpt_app.py).

The front-end interface is customizable based on your usecase and creating complex workflows totally depends on your comfort with Python and Streamlit. To understand in detail how you can customize a streamlit application, please refer to the following docs.

Following are the snapshots from the example application that is hosted here using streamlit sharing;

# Figure 3-2. Dashboard

# Figure 3-3. GPT-3 Sandbox

In the next chapters, we will explore different areas involving applications of GPT-3 and leverage this sandbox to create easily deployable web applications that can be shared with the GPT-3 community and the broader audience.

## Going Live with GPT-3-Powered Applications

By now you have successfully created your gpt-3 powered application and it’s time to put that into production. Just like every coin has two sides, there are a number of harmful ways in which GPT-3 can be used as discussed in section 1.5.1 (Risks associated with GPT-3). In order to safeguard the use of API against those malicious practices, OpenAI created a certain set of guidelines and procedures that you have to follow before going live with your application.

Currently, OpenAI allows you to serve the API out to five people without taking pre-approval, but as you grow to more than five people you need to apply for the production review. It is highly recommended to go through the Use Case Guidelines before even applying for the pre-launch review.

When you are ready to “go live” with your application, you can submit the Pre-Launch Review Request which will prompt the OpenAI team to look into your use case in detail and raise any potential red flags which may violate the API usage Safety Guidelines set up by OpenAI. When you apply for the pre-launch review, the openAI team examines how you have integrated the API into your application and assesses possible risks and the possibility of misuse.

After submitting the pre-launch review proposal, your request is either approved or denied, with appropriate feedback highlighting the potential red flags. If your request is successful, OpenAI will grant you a maximum spend limit, and increase that limit over time as you build a track record with your application. This gives you the freedom to build and deploy your application, while they monitor the potential impact of your application on the platform. As your application grows and builds up the user base, you can submit a Quota Increase Request for OpenAI to accommodate your application needs.

