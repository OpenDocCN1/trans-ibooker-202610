
- Fine-tune the existing models with the new training data
- Use the fine-tuned model

## Prepare and upload training data

Training data is what the model takes in as input for fine-tuning. Your training data must be a JSONL document, where each line is a prompt-completion pair corresponding to a training example. For model fine-tuning you can provide an arbitrary number of examples where it is highly recommended to create a value-targeted dataset to provide the model with quality data and wide representation. Fine-tuning improves performance with more examples, so the more examples you provide, the better the outcome.

Your JSONL document should look something like this:

```
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
...
```

Where prompt text should include the exact prompt text you want to complete, and ideal generated text should include an example of desired completion text that you want GPT-3 to generate.

You can use OpenAI’s CLI data preparation tool to easily convert your data into this file format. CLI data preparation tool accepts files in different formats, with the only requirement that they contain a prompt and a completion column/key. You can pass a CSV, TSV, XLSX, JSON or JSONL file, and it will save the output into a JSONL file ready for fine-tuning. To do that, you can use the following command:

```
openai tools fine_tunes.prepare_data -f <LOCAL_FILE>
```

Where LOCAL_FILE is the file you prepared for conversion.

## Train a new fine-tuned model

Once you prepare your training data as described above, you can move on to the fine-tuning job with the help of the OpenAI CLI. For that, you need the following command:

```
openai api fine_tunes.create -t <TRAIN_FILE_ID_OR_PATH> -m
<BASE_MODEL>
```

Where BASE_MODEL is the name of the base model you’re starting from (ada, babbage, or curie). Running this command does several things:

- Uploads the file using the files endpoint (as discussed earlier in this chapter);
- Fine-tunes the model using the request configuration from the command;
- Streams the event logs until the fine-tuning job is completed.

Log streaming is helpful to understand what’s happening in real-time and to respond to any incidents/failures as and when they happen. The streaming may take from minutes to hours depending on the number of jobs in the queue and the size of your dataset.

## Using the fine-tuned model

Once the model is successfully fine-tuned, you can start using it! You can now specify this model as a parameter to the Completion Endpoint, and make requests to it using the Playground.

> NOTE
> 
> After the fine-tuning job completes, it may take several minutes for your model to become ready to handle requests. If completion requests to your model time out, it is likely because your model is still being loaded. If this happens, try again in a few minutes.

You can start making requests by passing the model name as the model parameter of a completion request using the following command:

```
openai api completions.create -m <FINE_TUNED_MODEL> -p
<YOUR_PROMPT>
```

The FINE_TUNED_MODEL field will be populated with the name of the model. YOUR_PROMPT will be populated with the prompt you want to complete in this request.

You can continue to use all the Completion Endpoint parameters that were discussed in this chapter, like temperature, frequency_penalty, presence_penalty, etc., on these requests to the newly fine-tuned model as well.

NOTE
No engine is specified on these requests. This is the intended design and something that OpenAI plans on standardizing across other API endpoints in the future.

For more information, please refer to fine-tuning documentation.

## Tokens

Before diving deeper into how different prompts consume tokens, let’s look more closely at what is a token.

We’ve told you that tokens are numerical representations of words or characters. Using tokens as a standard measure, GPT-3 can handle training prompts from a few words to entire documents.

For regular English text, 1 token consists of approximately 4 characters. It translates to roughly ¾ of a word, so for 100 tokens there will be approximately 75 words. As a point of reference, the collected works of Shakespeare consist of about 900,000 words, roughly 1.2M tokens.

To maintain the latency of API calls, OpenAI imposes a limit of 2,048 tokens (approximately ~1,500 words) for prompts and completions.

To further understand how the tokens are calculated/consumed in the context of GPT-3 and to stay within the limits set by the API let us walk you through the following ways you can measure the token count.

In the *Playground*, as you enter text into the interface, you can see the token count update in real-time in the footer at the bottom right. It displays the number of tokens that will be consumed by the text prompt after hitting the submit button. You can use it to monitor your token consumption every time you interact with the Playground (see figure 2-8).

![](img/495f94a597dee9cc5b6b1e9654aa4339_53_0.png)

Figure 2-8. Token Count in the Playground

The other way to measure the consumption of tokens is by using the explicit *GPT-3 Tokenizer Tool* that lets you visualize the formation of tokens from the word characters. You can interact with the Tokenizer tool via a simple text box where you write the prompt text and Tokenizer will show you with the token and character counts along with the detailed visualization.

## Tokenizer

The GPT family of models process text using tokens, which are common sequences of characters found in text. The models understand the statistical relationships between these tokens, and excel at producing the next token in a sequence of tokens.

You can use the tool below to understand how a piece of text would be tokenized by the API, and the total count of tokens in that piece of text.

```
GPT-3 is the most powerful language model
```

Tokens
10
Characters
41

```
GPT-3 is the most powerful language model
```

A helpful rule of thumb is that one token generally corresponds to ~4 characters of text for common English text. This translates to roughly ¾ of a word (so 100 tokens ~= 75 words).

Figure 2-9. Tokenizer by OpenAI

For integrating the token count metric in your API calls to different endpoints, you can patch the logprobs and echo attribute along with the API request to get the full list of tokens consumed.

In the next section we will cover how tokens are priced based on the different execution engines.

## Pricing

In the last section we talked about tokens, which is the smallest fungible unit used by OpenAI to determine the pricing for API calls. Tokens allow greater flexibility than measuring the number of words or sentences used in the training prompt and due to token’s sheer granularity tokens can be easily processed and used to measure the pricing for a wide range of training prompts.

Everytime you call the API from either the Playground or programmatically, behind the scenes the API calculates the number of tokens used in the training prompt along with the generated completion and charges each call on the basis of the total number of tokens used.

At the moment of writing this chapter (Dec, 2021) the pricing for various API engines was the following:

| Model | Price Per 1k Tokens |
|---|---|
| Davinci (Most powerful) | $0.0600 |
| Curie | $0.0060 |
| Babbage | $0.0012 |
| Ada (Fastest) | $0.0008 |

The company works on the cloud pricing model of “pay as you go”. For updated pricing please check: https://beta.openai.com/pricing.

OpenAI generally charges a flat fee per 1k tokens and the fee may vary depending on the type of execution engine used in the API call. Davinci is the most powerful and expensive, while Curie, Ada and Babbage are cheaper and faster.

Instead of monitoring the tokens for each of the API calls, OpenAI provides a reporting dashboard to monitor daily cumulative token usage. Depending on your usage, it may look something like Figure 2-10.

## Usage

![](img/495f94a597dee9cc5b6b1e9654aa4339_56_0.png)

Figure 2-10. API Usage Dashboard

In figure 2-10 you can see a bar graph showing the daily token consumption from the API usage. The dashboard helps you to monitor the token usage and pricing for your organization. This helps you to regulate the API usage and stay within your budget. There is also an option to monitor the cumulative usage and breakdown of token count per API call. This should give you enough flexibility to create policies around token consumption and pricing for your organization. Now that you understand the ins and outs of the Playground and the API, we will take a look at GPT-3’s performance on typical language modelling tasks.
