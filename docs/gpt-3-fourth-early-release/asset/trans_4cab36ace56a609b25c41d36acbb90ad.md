

category is what I call “prompt-based development”. Companies going that path cover use cases such as generating copy for ads and websites. The design philosophy is relatively simple: the company takes your data in, sends it over into a prompt, and displays the API generated result. It solves a task that is easily done by a single API prompt and wraps an UI around that to deliver it to the users,” he shares.

The problem Erickson sees with this category of use cases is that it is already overcrowded, attracting many ambitious startup founders competing with services having similar USP. Instead, Erickson recommends looking at another category of use cases instead, following in the footsteps of Viable. Data driven use cases are not as crowded as prompt-generation use cases, and at the same time they are more profitable and allow you to easily create a moat. The key is to build a large set of data that you can keep adding to and that can provide potential insights. GPT-3 will help you extract valuable insights from it. At Viable, this was the model that let them monetize easily. “People pay a lot more money for data than they do for prompt output,” Erickson explains.

OpenAI @OpenAI · Mar 26
In the 9 months since we launched GPT-3, developers have created extraordinary apps using our API. Today GPT-3’s search, conversation, and text completion powers 300 applications focused on new ideas from productivity and education to creativity and games.

![](img/495f94a597dee9cc5b6b1e9654aa4339_21_0.png)

GPT-3 Powers the Next Generation of Apps
Over 300 applications are delivering GPT-3-powered search, conversation, text completion, and other advanced AI features through our API.
openai.com

Figure 1-4. Tweet from OpenAI

## Accessing the OpenAI API

As of 2021, the market has already produced several proprietary AI models that have more parameters than GPT-3. However, access to them is limited to a handful of people within the company’s R&D walls, making it impossible to evaluate their performance on real-world NLP tasks.

Another factor that makes GPT-3 accessible is its simple and intuitive “text-in, text-out” user interface. It doesn’t require complex gradient fine-tuning or updates, and you don’t need to be an expert to use it. This combination of

scalable parameters and relatively open access makes GPT-3 the most exciting, and arguably the most relevant language model to date.

Due to GPT-3’s extraordinary capabilities, there are significant risks in terms of security and misuse associated with making it open-source which we will cover in the last chapter. Keeping that into account OpenAI decided not to release the source code of GPT-3 publicly and came up with a unique, never seen before access sharing model via an API.

The company decided to initially release access to the API in the form of a limited beta user list. There was an application process in place, where people had to fill in a form detailing their background and the reasons for requesting the API access. Only the approved users were granted access to a private beta of the API with an interface called Playground.

In its early days, the waitlist for GPT-3 beta access consisted of tens of thousands of people. OpenAI swiftly managed the applications that started pouring in and adding developers in batches, but also closely monitoring their activity and feedback about the API user experience in order to continuously improve it.

Thanks to the progress with safeguards, OpenAI removed the waitlist in November 2021. GPT-3 is now openly accessible via a simple sign-in. This is a great milestone in the history of GPT-3 and a highly requested move by the community. To get API access, simply go to the sign-up page, sign up for a free account, and start experimenting with it right away.

With free access to GPT-3, users initially get a pool of free credits that allow them to freely experiment with the API. The number of credits is equivalent to creating text content as long as 3 average length novels.

After you use your free credits, you start paying for the usage, or, when you have a justified need, you can request additional credits with the OpenAI API customer support.

OpenAI strives to ensure that API-powered applications are built responsibly. For that reason, they provide tools, best practices and usage

guidelines to help the developers bring their applications to production quickly and safely.

They have also created Content Guidelines to clarify what kind of content OpenAI API can be used to generate. To help developers ensure their applications are used for their intended purpose, prevent potential misuse, and adhere to the content guidelines, they offer a free content filter. OpenAI policy prohibits the use of the API in ways that do not adhere to the principles described in its charter, including content that promotes hate, violence, self-harm, that intends to harass, influence political processes, spread misinformation, spam content, and more.

Once you have signed up for an OpenAI account, you can move on to chapter 2, where we will discuss the different components of API, the GPT-3 Playground, and how to use the API to the best of its abilities for different use cases.

## Chapter 2. Using the OpenAI API

### A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the authors’ raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 2nd chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the authors at sandra.a.kublik@gmail.com and shubhamsaboo111@gmail.com.

Even though the OpenAI API is the most sophisticated and complex language model in the world, its capabilities are abstracted as a simple “text-in-text-out” interface to end users. This chapter will get you started with using that interface, Playground, and cover the technical nuances of the OpenAI API, because it is always the details that reveal the true gems.

### WARNING

In order to work through this chapter, you will need to sign up for an OpenAI account at https://beta.openai.com/signup. If you haven’t done that, please do so now.

## Navigating the OpenAI Playground

Your OpenAI developer account provides access to the API and infinite possibilities. We’ll start with the Playground, a private web-based sandbox environment that allows you to experiment with the API, learn how its components work, and access developer documentation and the OpenAI community. We will then show you how to build robust prompts that generate favorable responses for your application. We’ll finish the chapter with examples of GPT-3 performing four NLP tasks: classification, named entity recognition (NER), summarization, and text generation.

In an interview with Peter Welinder, VP of Product at OpenAI we asked about the key advice on navigating the Playground for first-time users. He told us his advice depends on the persona of the user. If the user has a machine learning background, Peter encourages them to “start by forgetting the things that they already know, and just go to the playground and try to get GPT-3 to do what you wanted it to do by just asking it”. He suggests users “imagine GPT-3 as a friend or a colleague that you’re asking to do something. How would you describe the task that you want them to do? And then, see how GPT-3 responds. And if it doesn’t respond in the way that you want, iterate on your instructions.”

As YouTuber and NLP influencer Bakz Awan puts it, “The non-technical people ask: Do I need a degree to use this? Do I need to know how to code to use it? . . . Absolutely not. You can use the Playground. You don’t need to write a single line of code. You’ll get results instantly. Anybody can do this.”

NOTE
Before you start using the Playground, we recommend reading OpenAI’s “Getting Started” guide and the developer documentation. You’ll be able to access it with your OpenAI account.

- 1. Log in at https://openai.com. After you’ve authenticated, navigate to the Playground from the main menu.
- 2. Take a look at the Playground screen (Figure 2-1).

