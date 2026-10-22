
## Advice for Enterprises

Enterprises investigating the Azure OpenAI Service, Divakaruni says, can approach it much as they would investigating any other cloud service: you start with what makes the most sense for you and then look to see if the various technologies meet your needs. “While the technology is cool and that certainly has a wow factor, you still have to start with, ‘where can this be most applicable for me as a business, for my group?’ And then look to solve that with a set of technologies.”

The next step is to examine how to get from experimentation into production: “What are the other things that you need to build?” Divakaruni refers to this step as an “application glue that someone needs to inject around, making sure these models actually behave and can be used in a live application scenario”. That’s a nontrivial task, but enterprises need to think about this to understand what kind of investment a GPT-3-based application will require. Divakaruni advises asking, “Is this model actually producing things that are relevant when you have automation around? The use of the capability, when it’s actually built into an application—is it doing what it’s supposed to be doing?”

## OpenAI or Azure OpenAI Service: Which should you use?

The question for companies interested in exploring GPT-3, then, is: OpenAI API or Azure OpenAI Service? Divakaruni maintains that the OpenAI API version is more suitable for companies that are exploring their options but don’t have any specific project implementation in mind. In terms of access, OpenAI is definitely further along, with its Playground making it easier for individual users and companies to experiment there. The OpenAI API also allows access to the latest experimental models and API endpoints that expand the API’s capabilities.

Azure OpenAI Service, on the other hand, is targeting a cohort of users with production use cases who “graduate” from OpenAI API, or need to meet different compliance and privacy regulations. The two organizations encourage customers to experiment, and validate their use cases and firm them up with OpenAI API. If that platform meets their needs, Microsoft is encouraging the customers to stay on OpenAI API, but when their production needs become more mature and they start to need more compliance, they should consider transitioning to Azure.

## Conclusion

In this chapter, you saw how corporations are using GPT-3 based products at scale and how the new Microsoft Azure OpenAI service is paving the way for enterprises interested in becoming part of the GPT-3 ecosystem. We have dived into the nuances of scaling a GPT-3 powered product and shared some tips from the journey of large-scale enterprise-grade products. In Chapter 6, we will look at some of the controversies and challenges surrounding the OpenAI API and LLMs more generally.

1 The European Union’s General Data Protection Regulation requirements, prohibit companies from hiding behind illegible terms and conditions that are difficult to understand. It requires companies to clearly define their data privacy policies and make them easily accessible.

## Chapter 6. Challenges, Controversies, and Shortcomings

## A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the authors’ raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 6th chapter of the final book. Please note that the GitHub repo will be made active later on.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the authors at sandra.a.kublik@gmail.com and shubhamsaboo111@gmail.com.

Every technological revolution brings controversy. In this section we focus on four of the most controversial aspects of GPT-3: AI bias being encoded into the model; low-quality content and the spread of misinformation; GPT-3’s environmental footprint; and data privacy issues. When you mix human biases with a powerful tool capable of producing huge quantities of seemingly coherent text, the results can be dangerous. Please note that some of the examples and links in this chapter contain disturbing content related to racism and suicide.

The fluency and coherence of much of GPT-3’s text output raises several risks, because people are prepared to interpret it as meaningful. Many also view the human developers involved in creating GPT-3-based apps as

“authors” of its output and demand that they be held accountable for its contents.

The risks we consider in this chapter follow from the nature of GPT-3’s training data, which is to say, the English-speaking internet. Human language reflects our worldviews, including our biases—and people who have the time and access to publish their words online are often in positions of privilege with respect to racism, gender, and other forms of oppression, which means they tend to be overrepresented in LLM training data. In short, society’s biases and dominant worldviews are already encoded in the training data. Without careful fine-tuning (more on this later in the chapter), GPT-3 absorbs these biases, problematic associations, and violent abuse and includes them in its output for the world to interpret.

Whatever biases appear in the initial training set or user input are repeated and can be amplified or even radicalized by the GPT-3-generated output. The risk is that people read and spread such texts, reinforcing and propagating problematic stereotypes and abusive language in the process. Those targeted by the harmful messages may experience psychological repercussions. In addition, those wrongly perceived to be “authors” of the GPT-3-generated text may face harm to their reputations or even attempts at retribution. What’s more, such biases can also emerge in future LLMs trained on datasets that include the publicly available output of previous generations of LLMs. The sections that follow look more closely at some of these controversies.

## The Challenge of AI Bias

## RECOMMENDED READING

There are a number of O’Reilly books focused on the subject of AI bias which we encourage you to check out, among them titles such as *Practical Fairness* and *97 Things About Ethics Everyone in Data Science Should Know*.

Research has established that all LLMs have some sort of encoded human bias, including stereotypes and negative sentiment towards specific groups (especially marginalized minorities). One highly publicized research paper found that “the mix of human biases and seemingly coherent language heightens the potential for automation bias, deliberate misuse, and amplification of a hegemonic worldview.”¹

As YouTuber Kilcher notes, working with GPT-3 “is a little bit like interacting with all of humanity” because it’s been trained on datasets that represent a large swath of the internet, “which is like a skewed subsample of humanity.” LLMs amplify any biases in the datasets on which they are trained. Unfortunately, like much of humanity, this “skewed subsample of humanity” is rife with toxic biases, including gender, race, and religious prejudices.

A 2020 study of GPT-2, GPT-’s predecessor, found in the training data 272,000 documents from unreliable news sites and 63,000 from banned subreddits.² In the same study, both GPT-2 and GPT-3 showed a tendency to generate sentences with high toxicity scores, even when prompted with non-toxic sentences. OpenAI researchers noted early on that biased datasets led GPT-3 to place words like “naughty” or “sucked” near female pronouns and “Islam” near words like “terrorism”. Similarly, Stanford University researcher Abubakar Abid details consistent and creative biased tendencies in text generated by GPT-3, like associating the word “Jews” with “money” and “Muslim” with “terrorist.”³

Philosopher AI is a GPT-3 powered chatbot and essay generator, created to showcase the astounding capabilities of GPT-3, as well as its limits. A user enters any prompt, from a few words to a few sentences, and the app turns the fragment into a full essay of surprising coherence. Users quickly found, however, that certain types of prompts returned offensive and deeply troubling results.

For example, AI researcher Abeba Birhane prompted Philosopher AI to generate an essay on “what ails Ethiopia.” She summarized the surprising results in a tweet:
