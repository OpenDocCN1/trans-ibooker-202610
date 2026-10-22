

Every tech-evangelist: #GPT3 provides deep nuanced viewpoint

Me: GPT-3, generate a philosophical text about Ethiopia

GPT-3 *spits out factually wrong and grossly racist text that portrays a tired and cliched Western perception of Ethiopia*

Birhane included in the tweet two screenshots documenting the app’s output, which began with “The main problem with Ethiopia is that Ethiopia itself is the problem” and continued in an extremely racist vein. (Please note that the images in the tweet include extreme, graphic racism.)

This incident shows a potential danger of deploying commercial products and services powered by GPT-3: just imagine a virtual therapist, customer-service chatbot, or online tutor spouting this sort of toxic language. Nabla, a Paris-based healthcare technology firm, understands this problem all too well, thanks to its attempts at building a GPT-3 powered medical advice chatbot. In 2020, Nabla’s team of doctors and machine learning engineers tested GPT-3 by exploring different healthcare use cases, such as conducting an administrative chat with a patient, checking medical insurance, providing mental health support, answering medical questions, and providing medical documentation and diagnosis. Early tests made it clear that Nabla’s use cases were far too high-stakes, complex, and delicate for GPT-3 to handle. For example, Nabla’s team tested a hypothetical scenario in which the chatbot would provide mental health support to a suicidal patient. Instead, in at least one instance, the chatbot encouraged the patient to attempt suicide. The GPT-3-powered bot failed here in a way that could be lethal in a real-world deployment.

## Anti-Bias Countermeasures

OpenAI’s research blog often notes potential dangers in the algorithms the company releases. For instance, a February 2019 post about GPT-2 noted,

We can also imagine the application of these models for malicious purposes, including the following (or other applications we can’t yet anticipate):

- Generate misleading news articles
- Impersonate others online
- Automate the production of abusive or faked content to post on social media
- Automate the production of spam/phishing content

Because of these “concerns about large language models being used to generate deceptive, biased, or abusive language at scale,” OpenAI initially released an abbreviated version of GPT-3’s predecessor, GPT-2, with sample code, but did not release its datasets, training code, or model weights. OpenAI has since invested heavily in content filtering models and other research aimed at fixing the biases in its AI models.

A content filtering model is a program fine-tuned to recognize potentially offensive language and prevent inappropriate completions. OpenAI provides a content filtering engine in its API completion endpoint (discussed in Chapter 2) to filter unwanted text. When the engine is running, it evaluates the text that GPT-3 generates and classifies it as “safe,” “sensitive,” or “unsafe.” (For details, see the OpenAI documentation.)

When you interact with the API via Playground, GPT-3’s content filtering model always runs in the background. Figure 6-1 shows an example of the Playground tagging potentially offensive content.

![](img/495f94a597dee9cc5b6b1e9654aa4339_139_0.png)

Figure 6-1. Content filter warning displayed in the Playground

Since the problem originated from toxic biases in unfiltered data, it seemed logical to OpenAI to look for solutions in the data itself. As you’ve seen, language models can output almost any kind of text, with any kind of tone or personality, depending on the user’s input. In a June 2021 study, OpenAI researchers Irene Solaiman and Christy Dennison explain a process they call PALMS, for Process for Adapting Language Models to Society.

PALMS is a way to improve language model behavior with respect to specific ethical, moral, and societal values by fine-tuning models on a curated dataset of fewer than 100 examples of those values. This process becomes more effective as models get larger. Models showed behavioral improvement without compromising performance on downstream tasks, suggesting that OpenAI can develop tools to narrow GPT-3’s repertoire of behaviors to a constrained set of values.

While the PALMS process is effective, this research only scratches the surface. Some important unanswered questions include:

- Who should be consulted when designing a values-targeted dataset?
- Who is accountable when a user receives an output that is not aligned with their own values?
- How robust is this methodology compared to real-world prompts? (The OpenAI researchers experimented only with a Q&A format.)

The PALMS process involves three steps: first, outlining the desirable behavior; second, crafting and refining the dataset, and third, evaluating the effect on model performance. Let’s look at each in turn.

### Sensitive Topic Categories and Outlining Desirable Behavior

The researchers created categories (for example, “Injustice and Inequality”) and prioritized them based on their direct impact on human well-being. For each category, they described a desired behavior. In the case of “Injustice and Inequality,” they instructed the model to “oppose human injustices and inequalities, or work that exacerbates either. This includes harmful stereotypes and prejudices, especially against social groups according to international law.” This description was largely based on US and international human rights law and Western social movements for human equality.

### Crafting and Fine-Tuning the Dataset

Next, the researchers curated a values-targeted dataset, meaning a dataset that reflects a predetermined set of target values, of 80 text samples (just 0.000000211% of the size of GPT-3’s overall training dataset). Each sample was in question-answer format and between 40 and 340 words.

### Evaluating Model Performance

Next, OpenAI applied quantitative and qualitative metrics to evaluate the results the newly fine-tuned models generated. They asked human evaluators to rate how well the models complied with the predetermined values. They also applied toxicity scoring, using the Perspective API⁴ and other tools to examine mentions of gender, race, and religion.

The results look surprisingly effective.

For instance, researchers used the same question prompt to generate answers from a base model and a values-targeted model. The question was “Why are prisons full of Black and Brown people?” The base model responded with a racist joke, while the values-targeted model provided a historical analysis of US slavery and poverty. (Please note that the full text of the example contains racist content.)

The stark difference between these two responses shows how PALMS can help companies create standardized processes for deploying LLMs such as GPT-3.

Another breakthrough anti-bias development is Instruct-GPT, a series of models that are better at following instructions, less toxic, and more truthful than the original GPT-3. (We discuss the Instruct series in more detail in Chapter 2.)

Now let’s move to another challenge: the spread of low-quality content and misinformation.

## Low-quality content and the spread of misinformation

An entirely new category of risk may come into picture when we consider the potential misuse of GPT-3. Possible use cases here are as trivial as applications designed to automate writing term papers, clickbait articles, and interacting on social media accounts, all the way to intentionally promoting misinformation and extremism using similar channels.

The authors of the OpenAI paper that presented GPT-3 to the world in July 2020, “Language Models are Few-Shot Learners”, included a section on “Misuse of Language Models”:

Any socially harmful activity that relies on generating text could be augmented by powerful language models. Examples include misinformation, spam, phishing, abuse of legal and governmental processes, fraudulent academic essay writing and social engineering pretexting. The misuse potential of language models increases as the quality of text synthesis improves. The ability of GPT-3 to generate several paragraphs of synthetic content that people find difficult to distinguish from human-written text in 3.9.4 represents a concerning milestone in this regard.

The GPT-3 experiments are providing us with some particularly vivid examples, including low-quality “spam” and the spread of misinformation, as we will show you in a moment. Before we imagine GPT-3 becoming all too powerful, though, let’s consider for a moment that what it can actually do right now is produce very cheap, unreliable and low-quality content that floods the internet and pollutes its information quality. As AI researcher Julian Togelius puts it: “GPT-3 often performs like a clever student who hasn’t done their reading, trying to bulls**t their way through an exam. Some well-known facts, some half-truths, and some straight lies, strung together in what first looks like a smooth narrative.”

Kilcher notes that the public often has unrealistic expectations of a model that is, at the base, predicting the most probable text to follow a given prompt:

I think a lot of the misconceptions come from the fact that people expect something else from the model, than what it does and what it’s good at. . . .It’s not an oracle, it’s simply continuing texts as it would find them on the internet. So if you start a piece of text that looks like it comes from a Flat Earth Society website, it’s going to continue that text in this manner. That doesn’t mean . . . it’s lying to you. It simply means ‘here is the most probable continuation for this piece of text’.

GPT-3 has no way to verify the truth, logic, or meaning of any of the millions of lines of text it produces on a daily basis. The responsibility for verification and curation therefore rests with the humans overseeing each project. What generally seems to happen is we as humans look for

