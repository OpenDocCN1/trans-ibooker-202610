# NOTE

In Go, whenever you build reusable pieces of code, you will develop a package as a shared library. But when you develop executable programs, you will use the package “main” to make the package an executable program. By calling this package the main function in the package “main” will be set up as the entry point of our executable program.

After importing the dependencies and packages, let’s create a main function that will host the entire logic of reading the training prompt providing us with the completion output. We will use the following configuration for the API parameters:

```
Maximum tokens=100, Execution Engine="davinci",
Temperature=0.5, Top-p=1, Frequency Penalty = 0.2,
Stop Sequence = ["\n\n"]
```

```
func main() {
    c := gogpt.NewClient("OPENAI-API-KEY")
    ctx := context.Background()
    prompt, err :=
    ioutil.ReadFile("prompts/summarize_for_a_2nd_grader.txt")
    req := gogpt.CompletionRequest{
        MaxTokens: 100,
        Temperature: 0.5,
        TopP: 1.0,
        Stop: []string{"\n\n"},
        FrequencyPenalty: 0.2,
        Prompt: string(prompt),
    }
    resp, err := c.CreateCompletion(ctx, "davinci", req)
    if err != nil {
        return
    }
    fmt.Println("----------------------")
    fmt.Println(resp.Choices[0].Text)
    fmt.Println("----------------------")
}
```

In the code we perform the following tasks step-by-step:

+   1. We start with setting up a new API client by providing it with the API token and then we put it to run in the background.