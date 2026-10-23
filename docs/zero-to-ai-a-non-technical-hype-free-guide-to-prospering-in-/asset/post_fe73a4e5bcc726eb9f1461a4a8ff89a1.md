
high, as we’re asking our algorithm to come up with a sentence that describes a potentially complex combination of drugs.

The challenges in working with this kind of data were recognized by Dr. Amy Abernethy, chief medical officer at Flatiron Health and former director of cancer research at the Duke Cancer Institute. In an interview for a paper published in the 2017 Journal of the National Cancer Institute, she stated, “The MD Anderson experience is telling us that solving data quality problems in unstructured data is a much bigger challenge for artificial intelligence than was first anticipated.”

One way to simplify things is to reduce depth by posing the problem as a classification task dealing with structured data. Another is to reduce width by limiting the application to a specific kind of cancer. By applying both measures, instead of feeding the algorithm with wordy sentences filled with medical jargon, they would have used a table with patient information expressed in values (for example, “blood pressure = 110 mm Hg”). The algorithm would have responded by choosing one out of a number of suggested therapies known in advance. A project like this is certainly less cool than an all-knowing AI that responds in natural language, but we argue that it’s better to have something simple that works than something complex that doesn’t.

### 9.5.4 Emotional diary

Finally, let’s cover two examples from our consulting work that you won’t find in the news. We changed some details to protect the privacy of our clients and respect our confidentiality agreements, but the lessons are still valid.

The first example is actually more of an averted disaster. A health-care company wanted to build an app to give emotional support to couples undergoing fertility treatment. Not only are these couples stressed out because they have difficulty conceiving, but the woman is also prescribed hormones that can have a strong impact on her mood.

The original idea was to build an “emotional diary” app for the couple. Every day, it would ping them to upload a selfie with a smile or a sad face. An algorithm would recognize their mood and answer with an appropriate motivating sentence. At the end of the therapy, the app would have created a “diary” of the emotional journey that the couple went through, hopefully ending with the beautiful gift of a child.

The company had already started to collect quotes from various technological providers and had come up with a marketing plan to promote the app across treatment centers. One morning, we discovered that Microsoft offers an “emotional detection API”: a simple service that allows you to send a selfie to Microsoft’s servers, which run an AI algorithm and return their evaluation of the emotion expressed by the person. We asked to pause the strategy for a day. The day after, we came back with a prototype app linked to Microsoft’s service, which already included a few sentences to encourage the couple.

We took our bare-bones prototype and went to a fertility center to talk to some users. The verdict was unanimous: the women wanted to throw that phone at a wall. What we thought would be a supportive tool was instead seen as a disrespectful invasion of their privacy and intimacy. They were already stressed and doubtful they would ever become mothers; the last thing they wanted was an app that wanted selfies to send cheeky motivational quotes.

We canceled the program and went back to the drawing board. Fast prototyping and the use of APIs allowed us to find out early in the process that we were about to go full speed right into a wall.

### 9.5.5 Angry phone calls

Another example is a company that needed an AI algorithm that could identify emotions from voice recordings. During a phone call, the app should indicate when either speaker is angry, sad, happy, or neutral and provide suggestions to deal with the negative feelings. Our client came to us claiming that they had already found a technology provider that was reliable, and we dove head-down into execution without questioning the technology. Together, we drafted a comprehensive strategy, a business plan, and designed a minimum viable product (MVP) that integrated the third-party technology. Once we shipped the MVP, we found out that the performance of the tech provider was nowhere close to what they claimed (and we expected).

Apparently, the software was designed around a dataset of German speakers. We needed to apply it to the Italian market, and it turns out that an angry German and an angry Italian sound quite different. After acknowledging that the vendor was underperforming, we dug into the scientific literature to learn about the state-of-the-art performance in this task. We found out that even the state of the art was far away from the 90%-plus accuracy achieved in other AI tasks. Lesson learned? Test, test, test. Always question and validate what you read. Even if a technology provider claims to have a great product, spend one or two days to research the state of the art and to test whether it can reach the performance you need.

### 9.5.6 Underperforming sales

In another example, a large corporation asked us to cluster its sales data to spot underperforming customer accounts. The request came from the IT department, which managed large amounts of data and wanted to prove the value of that data collection effort. We ran our analysis and found a bunch of underperforming stores through which the company could have made $8.5 million more per year by changing the sales strategy! Everyone was excited, so we crafted a beautiful presentation for the CEO. He interrupted us after just a few minutes saying: “Guys, you’re not telling me anything new. We made a special deal 10 years ago with these stores you call underperforming; I can tell you that the profit is very high with them. You’re looking at the wrong metrics.”

It turned out that the stores in question had custom volume discounts that were booked differently, and therefore didn’t show up in the database we used. What do we learn from this story? Always include the business perspective when designing an AI project. Don’t fall into the trap of designing AI projects with a tech-first approach: make sure the business value is there before thinking about algorithms.

You probably noticed that none of the solutions to these fledgling projects was to invest in better technology. The hero they needed is not a super-skilled data scientist, but an enlightened leader who understands the principles we covered in this book and has the domain knowledge, critical spirit, and drive to bring AI into an organization.

Congratulations! You basically (almost) made it to the end of the book! Part 2 covered a lot of material with real-life situations and stories that will help you find and complete impactful AI projects in your organization. Together with the groundwork that we laid down in Part 1, you’re now ready to think critically about how AI can benefit your organization. Take a deep breath and get ready for the last chapter, where we take a step back and discuss how the AI tools you’ve been learning about will shape society in the future.

### Summary

- When building an AI project, you must decide whether to build technology, buy it from tech providers, or borrow it by using a mixture of third-party technology and your data.
- The Lean AI Strategy helps you minimize implementation risk by guiding you in the build/buy/borrow decision-making process.
- The AI virtuous cycle allows you to continuously improve your AI models by exploiting the new data you’re collecting with your product.
- AI isn’t a silver bullet. Countless companies have failed at AI, often because of poor strategy or lack of market interest in their AI-based products.

## 10 What lies ahead

### This chapter covers

- Possible negative implications of AI on society
- The most promising ways in which AI can reshape humanity
- The potential for disruption by AI in specific industries

This is the last chapter of the book. By now, you have all the tools to understand what AI is and what the most important families of algorithms can be useful for. We also shared with you the lessons we and industry leaders have learned through building AI projects. However, just as no mountain ascent is complete without stopping to look over the landscape, our journey wouldn’t be complete without a broader look at the future of AI. Thanks to your newfound knowledge about the technical and organizational aspects of AI, you now have the mindset you need to break through the hype and think critically about how this technology is going to shape society in the next 5 to 10 years.

The book focused on what AI can do within the boundaries of organizations. For this final chapter, please allow us to be a bit more philosophical as we broaden the horizons of our discussion to society as a whole. To give as balanced an outlook
