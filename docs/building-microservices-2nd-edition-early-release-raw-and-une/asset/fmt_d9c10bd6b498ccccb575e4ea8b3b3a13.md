
# Progressive Delivery

Jez makes the case that by separating these two ideas, we can ensure that our software works in its production setting without failures being seen by our users. Blue-Green deployments are one of the simplest examples of this concept in action - you have one version of your software live (blue), and then deploy a new version alongside the old version in production (green). You check to make sure that the new version is working as expected, and if it is you redirect customers to now see the new version of your software. If you find a problem before the switch-over, no customer is impacted.

While Blue-Green deployments are amongst the simplest examples of this principle - it turns out there are many more sophisticated techniques we can use when we embrace this concept.

## On To Progressive Delivery

James Governor, co-founder of developer-focused industry analyst firm RedMonk, first coined^([16](#ch07.xhtml#idm46534869506968)) the term Progressive Delivery to cover a number of different techniques being used in this space. He has gone on to describe Progressive Delivery as “continuous delivery with fine-grained control over the blast radius”^([17](#ch07.xhtml#idm46534869505032)) - so an extension of continuous delivery, but a technique that gives us the ability to control the potential impact of our newly released software.

Picking up this theme, Adam Zimman from LaunchDarkly described^([18](#ch07.xhtml#idm46534869502888)) how progressive delivery impacts “the business”. From that point of view, we require a shift in thinking about how new functionality reaches our customers. It’s no longer a single rollout - it can now be a phased activity. Importantly though, progressive delivery can empower business stakeholders, as Adam puts it “delegating the control of the feature to the owner that is most closely responsible for the outcome”. For this to work though, the stakeholders in question need to understand the mechanics of the progressive delivery technique being used. So this requires that our product owners and the like are more technically capable of understanding how best to use progressive delivery.

We’ve already touched on Blue-Green deployments as one progressive delivery technique. Let’s briefly take a look at a few more

## Feature Toggles

With Feature Toggles (otherwise known as Feature Flags), we hide deployed functionality behind a toggle which can be used to switch functionality off or on. This is most commonly used as part of trunk-based development, where functionality which isn’t yet finished can be checked in and deployed but still hidden from end users, but it has lots of applications outside of this. This could be useful to turn on a feature at a specified time, or turn off a feature which is causing problems.

You can also use feature toggles in a more fine-grained manner, perhaps allowing a flag to have a different state based on the nature of the user making a request. So you could for example have a group of customers that see a feature turned on (perhaps a beta test group), whereas most people see the feature as being turned off - this could help you implement a canary rollout, something we discuss next. Fully managed solutions exist for managing feature toggles, including LaunchDarkly^([19](#ch07.xhtml#idm46534869496888)) and Split^([20](#ch07.xhtml#idm46534869495336)). Impressive as these platforms are, I think you can get started with something much simpler - just a configuration file can do for a start, then look at these technologies as you start pushing how you want to use the toggles.

For a much deeper dive into the world of feature toggles, I can heartily recommend Pete Hodgson’s writeup “Feature Toggles (aka Feature Flags)”^([21](#ch07.xhtml#idm46534869493032)) which goes into a lot of detail regarding how to implement them, and the many different ways they can be used.

## Canary Release

> To err is human, but to really foul things up you need a computer^([22](#ch07.xhtml#idm46534869489304)).
>
> Paul Ehrlich

We all make mistakes, and computers can let us make mistakes faster and at larger scale than ever before. Given that mistakes are unavoidable (and trust me, they are) then it makes sense to do things which allow us to limit the impact of these mistakes. Canary Releases are one such technique.

Named for the canaries taken into mines as an early warning system for miners to warn them of the presence of dangerous gases, with a canary rollout the idea is that a limited subset of our customers see new functionality. If there is a problem with the rollout, then only that portion of our customers are impacted. If the feature works for that canary group, then it can be rolled out to more of your customers until everyone sees the new version.

For a microservice architecture, a toggle could be configured at an individual microservice level, turning functionality on (or off) for requests to that functionality from the outside world or other microservices. Another technique is to have two different versions of a microservice running side by side, and use the toggle to route to either the old or the new version. Here, the canary implementation has to be somewhere in the routing/networking path, rather than being in one microservice.

When I first did a canary release we controlled the rollout manually. We could configure the percentage of our traffic seeing the new functionality, and over a period of a week we gradually increased this until everyone saw the new functionality. Over the week, we kept an eye on our error rates, bug reports and the like. Nowadays, it’s more common to see this process handled in an automated fashion. Tools like Spinaker^([23](#ch07.xhtml#idm46534869483160)) for example have the ability to automatically ramp up calls based on metrics, for example increasing the percentage of calls to a new microservice version if the error rates are at an acceptable level.

## Parallel Run

With a canary release, a request to a piece of functionality will be served by either the old or the new version. This doesn’t though allow us to directly compare two different implementations. This could be important if you want to make sure that the new functionality works in exactly the same way against a known baseline. One of the easier ways to do this is to run any request against both the old and new implementations, and compare the result.

With a parallel run you do exactly that - you run two different implementations of the same functionality side by side, and send a request to the functionality to both implementations. With a microservice architecture, the most obvious approach might be to dispatch a service call to two different versions of the same service and compare the results. An alternative is to co-exist both implementations of the functionality inside the same service, which can often make comparison easier.

When executing both implementations, it’s important to realise that you likely only want the results of one of the invocations. One implementation is considered the source of truth - this is the implementation you currently trust, and is typically the existing implementation. Depending on the nature of the functionality you are comparing with a parallel run, you might have to give this nuance careful thought - you wouldn’t want to send two identical order updates to a customer, or pay an invoice twice for example!
