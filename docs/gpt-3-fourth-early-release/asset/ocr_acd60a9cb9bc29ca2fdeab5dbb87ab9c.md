![](bbox=[0.15, 0.09, 0.85, 0.38])

Figure 6-1. Content filter warning displayed in the Playground

Since the problem originated from toxic biases in unfiltered data, it seemed logical to OpenAI to look for solutions in the data itself. As you’ve seen, language models can output almost any kind of text, with any kind of tone or personality, depending on the user’s input. In a June 2021 study, OpenAI researchers Irene Solaiman and Christy Dennison explain a process they call PALMS, for Process for Adapting Language Models to Society.

PALMS is a way to improve language model behavior with respect to specific ethical, moral, and societal values by fine-tuning models on a curated dataset of fewer than 100 examples of those values. This process becomes more effective as models get larger. Models showed behavioral improvement without compromising performance on downstream tasks, suggesting that OpenAI can develop tools to narrow GPT-3’s repertoire of behaviors to a constrained set of values.

While the PALMS process is effective, this research only scratches the surface. Some important unanswered questions include:

+   - Who should be consulted when designing a values-targeted dataset?
+   - Who is accountable when a user receives an output that is not aligned with their own values?