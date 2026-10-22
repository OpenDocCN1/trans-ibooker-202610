stop sequence: whenever the API encounters that phrase, it will stop generating new tokens.

![](bbox=[0.14, 0.16, 0.85, 0.45])

Figure 2-6. Stop Sequence component

# Inject Start Text and Inject Restart Text

The *inject start text* and *inject restart text* parameters allow you to insert text at the beginning or end of the completion, respectively. You can use them to keep a desired pattern going. Often, these settings work in tandem with the stop sequence, as in our example. The prompt has the pattern where an English sentence is provided with the prefix “English:” (the restart text) and the translated output is generated with the prefix “French:” (the start text). As a result, anyone can easily distinguish between the two and create a training prompt that both the model and the user can clearly comprehend.

Whenever we run the model for such kinds of prompts, the model automatically injects a start text “French:” before the output and a restart text “English:” before the next input, so that this pattern can be sustained

# Show Probabilities