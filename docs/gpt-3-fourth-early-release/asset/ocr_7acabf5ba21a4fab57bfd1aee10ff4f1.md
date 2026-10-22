+   - How robust is this methodology compared to real-world prompts? (The OpenAI researchers experimented only with a Q&A format.)

The PALMS process involves three steps: first, outlining the desirable behavior; second, crafting and refining the dataset, and third, evaluating the effect on model performance. Let’s look at each in turn.

# Sensitive Topic Categories and Outlining Desirable Behavior

The researchers created categories (for example, “Injustice and Inequality”) and prioritized them based on their direct impact on human well-being. For each category, they described a desired behavior. In the case of “Injustice and Inequality,” they instructed the model to “oppose human injustices and inequalities, or work that exacerbates either. This includes harmful stereotypes and prejudices, especially against social groups according to international law.” This description was largely based on US and international human rights law and Western social movements for human equality.

# Crafting and Fine-Tuning the Dataset

Next, the researchers curated a values-targeted dataset, meaning a dataset that reflects a predetermined set of target values, of 80 text samples (just 0.000000211% of the size of GPT-3’s overall training dataset). Each sample was in question-answer format and between 40 and 340 words.

# Evaluating Model Performance

Next, OpenAI applied quantitative and qualitative metrics to evaluate the results the newly fine-tuned models generated. They asked human evaluators to rate how well the models complied with the predetermined values. They also applied toxicity scoring, using the Perspective API⁴ and other tools to examine mentions of gender, race, and religion.

The results look surprisingly effective.