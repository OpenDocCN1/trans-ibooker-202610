
that part does not involve any configuration or management of servers and is therefore *serverless*.

You are most likely already using serverless computing for some tasks. Consider three popular types of server:

- Web Servers
- Email Servers
- File Servers

Of these three, the second two are servers that we have been replacing with managed serverless solutions for a long time now. If you have ever sent an email via an API call to Mailchimp, Mailgun, SendGrid, SparkPost, or indeed Amazon’s Simple Email Service (SES) then you have used a serverless emailing solution. The days of running an SMTP server either in-house or in the cloud are, for a lot of organizations, already firmly in the past. File servers too are steadily going out of fashion and being replaced by serverless file storage solutions. Many modern web applications rely entirely on cloud native services such as Amazon Simple Storage Service (S3) as their primary storage mechanism for files. The last stone to fall from that list above is the web server. In this chapter we will show you how to replace your .NET web server with a serverless, cloud native implementation and what that means for the way you write your code.

## A Serverless Web Server

A web server is a computer, either physical or virtual, that is permanently connected to the internet and is ready to respond to HTTP(S) requests 24 hours a day. The lifecycle of an HTTP request inside a web server involves a lot of steps and executes code written by many different parties in order to read, transform and execute each request. Right in the middle of that journey is the code written by you, the application developer.

In order to replicate the functionality of a web server, we need a way to run that custom logic on a managed and on-demand basis. AWS offers us a few ways to do this, we will look at a service that allows us to deploy a fully-managed containerized application without having to worry about servers. AWS Fargate is another serverless solution for running web servers on a pay-as-you-go pricing model. These services take your web services and deploy them to the cloud in a serverless way. We can, however, go one step further and break apart the service itself into individual functions that are deployed independently to the cloud. For this we will need functions as a service (FaaS).

FaaS solutions, such as AWS Lambda, are stateless compute containers that are triggered by events. You upload the code you want to run when an event occurs and the cloud provider will handle provisioning, executing and then deprovisioning resources for you. This unlocks two main advantages:

- 1. FaaS allows your code to scale down to zero. You only pay for the resources you use *while your function is executing* and nothing in between. This means you can write applications that follow a true pay-as-you-go pricing model paying only for each function execution and not for the unused time in between.
- 2. FaaS forces you to think about separation of concerns, and places restrictions on the way you write your application that will generally lead to better code.

> *Simplicity is hard work. But, there’s a huge payoff. The person who has a genuinely simpler system - a system made out of genuinely simple parts, is going to be able to affect the greatest change with the least work. He’s going to kick your ass. He’s gonna spend more time simplifying things up front and in the long haul he’s gonna wipe the plate with you because he’ll have that ability to change things when you’re struggling to push elephants around.*
> 
> —Rich Hickey, creator of the Clojure programming language

Simplicity as it relates to writing your code for FaaS means to events, you want to keep point number 1 above true so that the sentence “when X happens perform Y” remains true for every time event X occurs. You also need to keep your function stateless (point 2 above). This is both a limitation and a feature of running your function on managed FaaS. Because AWS can and will deprovision resources in between executions of your function it is not possible to share static variables. If you want to persist data in an FaaS function you must intentionally persist it to a shared storage area, for example by saving to a database or a Redis cache.

Architecting your code into stateless functions with no side effects like this can be a challenge that requires discipline. It forces you to think about how to write your logic and how much scope each area of your code is really allowed to control. It also forces you to think about separation of concerns.

When multiple FaaS functions are connected together using events and augmented with other serverless services (such as message queues and API gateways), it becomes possible to build a back end application that will be able to perform any function a traditional web API running on a server can.

This allows you to write a completely serverless back end application hosted on AWS. And the best bit is, you can do all of this in C#! AWS offers many services you can use as the building blocks for such a serverless application, let’s take a look at some of the components they have to offer.

## Choosing Serverless Components for .NET on AWS

We are going to introduce you to some of the most useful serverless services from AWS and how you can use the various packages of the AWS SDK for .NET to interact with these services in your code. Let’s progressively build a serverless web application for the rest of this chapter, adding functionality with each new concept introduced.

Imagine that we run a software development consultancy and are recruiting for the best and brightest C# developers around! In order for them to apply, we want them to visit our website, where they will find an HTML form that allows them to upload their resume as a PDF file. Submitting this form will send their PDF resume to our API, where it will be saved to a file server and an email will be sent to our recruitment team, asking them to review it. We’ll call this the Serverless C# Resume Uploader. Figure 3-1 shows a high level overview of the architecture.

www.example.com

# Welcome to our Website!

Upload your resume here

Upload File...

PDF File

C#

Web API

S3 Bucket

Email

## Figure 3-1. Serverless C# Resume Uploader Architecture

The current implementation of our backend uses a web API controller action to accept the PDF file upload, save it to cloud storage and email our recruitment team. The code looks like this:

```
[ApiController]
[Route("[controller]")]
public class ApplicationController : ControllerBase
{
    private readonly IStorageService _storageService;
    private readonly IEmailService _emailService;

    public ApplicationController(IStorageService storageService, IEmailService emailService)
    {
        _storageService = storageService;
        _emailService = emailService;
    }

    [HttpPost]
    public async Task<IActionResult> SaveUploadedResume()
    {
        Request.EnableBuffering();
        using var fileStream = new MemoryStream();
        using var reader = new StreamReader(fileStream);
        await Request.Body.CopyToAsync(fileStream); ①

        var storedFileUrl = await
        _storageService.Upload(fileStream); ②

        await _emailService.Send("recruitment@example.com",
            $"Somebody has uploaded a resume! Read it here: {storedFileUrl}"); ③

        return Ok();
    }
}
```

① Read the uploaded file from the request.
② Save it to cloud storage.
③ Send an email to our recruitment team with a link to the file.

The code for IStorageService looks like this:
