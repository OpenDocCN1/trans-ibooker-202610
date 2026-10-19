
| `git` || A distributed version-control system that lets you track changes in your codebase, collaborate with others, and manage code across branches and repositories. |
| --- | --- |
| `tree` || A command-line utility that prints directory contents in a hierarchical tree structure, making it easier to inspect the layout of a project at a glance. |
| `npm` || The Node Package Manager used for installing, updating, and managing dependencies in modern JavaScript and TypeScript projects. |
| `npx` || A command-line tool that executes Node.js packages without requiring a global install—ideal for running project-specific tools and scripts. |
| `serve` || A lightweight static file server often used to locally preview production builds of web applications. |
| `aws` || The official AWS CLI tool, used for managing AWS services, running scripts to automate infrastructure, and interacting with cloud resources from the terminal. |
| `vite` || A fast frontend build tool and development server optimized for modern JavaScript frameworks like React. |
| `wget` || A utility used to download files over the network from the terminal. This utility is commonly used for scripting bulk downloads or fetching website content. |

### Note

This section assumes you are using your local machine, which will host the vulnerable-by-design serverless lab environment. While the frontend code for this serverless application would normally be served from an S3 bucket with public access and Static Website Hosting enabled, it is run locally here in this chapter to avoid exposing real credentials.^[17](#ch04.html#id577)

Now that we have discussed the prerequisites, let’s proceed by cloning the project repository and exploring the project structure:

1. Run the following command locally in a terminal window to clone the repository to your local machine:^[18](#ch04.html#id578)

```
GH_USERNAME=learning-serverless-security
GH_REPO=exposed-credentials-chat-example

git clone https://github.com/$GH_USERNAME/$GH_REPO.git
```

#### Note

The code samples used in this section can be found in the [`exposed-credentials-chat-example` GitHub repository](https://oreil.ly/RGfia).

2. Navigate into the project directory:

```
cd exposed-credentials-chat-example
```

3. Use `tree` to get a high-level view of the project’s structure:^[19](#ch04.html#id579)

```
tree .
```

This returns a tree-like file and folder structure:

```
.
├── README.md
├── eslint.config.js
├── index.html
├── package-lock.json
├── package.json
├── public
│   └── vite.svg
├── src
│   ├── App.css
│   ├── App.jsx
│   ├── index.css
│   └── main.jsx
└── vite.config.js
```

You will find the application’s core logic inside the `src/` directory.

The vulnerable-by-design application is built using React and Vite. To emulate the number of developers who now incorporate AI tools into their workflow, portions of the application were coded with the help of generative AI tools.

4. Remove the existing Git repository metadata by deleting the hidden `.git` directory:

```
rm -rf .git
```

Now that you have a better idea of how the application looks, let’s proceed with configuring and running it locally:

1. Open the `src/App.jsx` file in your preferred code editor and locate the following block of code:

```
const client = new BedrockRuntimeClient({
  region: "us-east-1",
  credentials: {
    accessKeyId: "",
    secretAccessKey: "",
  },
});
```

2. Update `src/App.jsx` by specifying the AWS access key ID and secret access key associated with the `super-admin-user` IAM user (with `AdministratorAccess`) you created earlier.

#### Tip

Make sure to save the `src/App.jsx` file before proceeding. Your changes won’t take effect until the file is saved, and your development server may not reflect the updated credentials otherwise.

3. In your terminal, run this command to install the project’s dependencies:

```
npm install
```

4. With everything ready, you can run the following command to launch the application locally:

```
npm run dev
```

You should see the following output in your terminal:

```
  VITE v6.3.5  ready in 394 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h + enter to show help
```

#### Caution

This example intentionally includes hardcoded AWS access credentials directly in the frontend source code. While running this vulnerable AI-powered application locally in a controlled environment is safe, exposing it through `--host` or deploying it to a public endpoint (such as via S3 static website hosting) could result in the credentials being leaked and compromised. Avoid deploying this version of the application outside your local development setup. In addition, never push this repository (or any version of it) containing credentials to a public Git repository—as this will expose the credentials to anyone able to access the repository. Doing so may lead to an immediate and complete compromise of your AWS account.

5. Open *http​://localhost:5173/* in a new browser tab to see the vulnerable AI-powered application in action.

[Figure 4-6](#ch04.html#vulnerable-serverless-chatbot-application) shows a basic chatbot interface with a chat window along with a chat input field for submitting your prompts or questions. Clicking the Send button submits your input prompt to the Amazon Bedrock runtime API. More specifically, the frontend code uses the AWS SDK for JavaScript to call the model, passing your input as part of a structured `messages` array. The application then updates the chat window after the API returns a generated response.

![](assets/lsse_0406.png)

6. To verify that the chatbot is working as expected, type the following question in the chat input field and then click Send:

```
Is it safe to hardcode AWS credentials in the frontend code?
```

After a few seconds, the chatbot should return a response along these lines:

```
Bot: No, it is not safe to hardcode AWS credentials in frontend code. This is a significant security risk and should be avoided for several reasons: 1. Exposure: Frontend code is visible to anyone who can access your website or application. This means your AWS credentials would be exposed to the public. 2. Credential compromise: Anyone with access to these credentials could potentially use them to access your AWS resources, incurring costs or causing damage to your infrastructure.
...
```

The chatbot is expected to respond within 30 seconds. If it doesn’t, open your browser’s developer tools and check the Console tab. If you see an `UnrecognizedClientException`, the AWS credentials you specified in the `src/App.jsx` file might be invalid or configured incorrectly.

#### Note

If you encounter the error `Model use case details have not been submitted for this account. Fill out the Anthropic use case details form before using the model. If you have already filled out the form, try again in 15 minutes.`, select and use the model (Anthropic > Claude 3.5 Sonnet v1) in the “Chat / Text playground” of Amazon Bedrock to open the “Submit use case details for Anthropic” form. Fill out and submit the form with the required information to register your intended use case and enable model access.

7. Once you’ve confirmed that the chatbot is working, it’s time to simulate a production deployment by building the application. Stop the running development server by pressing Ctrl-C (or Command-C on macOS). Run the following command to build your project:

```
npm run build
```

This yields the following output:
