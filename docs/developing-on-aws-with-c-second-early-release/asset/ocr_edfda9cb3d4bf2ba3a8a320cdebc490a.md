Figure 3-3. Testing your deployed Lambda function

The results of your execution are inserted into the page, including an inline “Log output” window which is also shown in Figure 3-3. This allows you to rapidly find and debug errors in your Lambda function and perform manual testing.

# C# Resume Example: AWS Lambda

Now that we are familiar with AWS Lambda, let’s apply this to our serverless example. We are going to take our web API controller from the C# Resume Uploader example and turn it into an AWS Lambda function. Any web API controller can be deployed to AWS Lambda with the help of a package from AWS called
Amazon.Lambda.AspNetCoreServer.Hosting. This package uses makes it easy to wrap a .NET controller in a Lambda function called by API Gateway

> NOTE
> 
> API Gateway is a service from AWS that makes building an API composed of Lambda functions possible. It provides the plumbing between HTTP requests and Lambda functions allowing you to configure a set of individual Lambda functions to run on GET, PUT, POST, PATCH and DELETE requests to API routes you configure in API Gateway. We are using API Gateway in this example to map the route POST:https://our-api/Application to a Lambda function.

To deploy an ASP .NET application to AWS Lambda using this method, install the Amazon.Lambda.AspNetCoreServer package then add a call to AddAWSLambdaHosting() in the services collection of the application⁴. This allows API Gateway and AWS Lambda to act as the web server when running on Lambda.

```
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorPages();
```