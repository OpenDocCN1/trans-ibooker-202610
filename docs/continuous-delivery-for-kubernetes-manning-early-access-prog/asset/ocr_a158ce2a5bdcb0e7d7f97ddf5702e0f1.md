Sooner or later, you will realise that there is no Application anymore, now you have a set of independent services that are evolving at a different pace.

Without having the right tests in place, you will spend weeks (including weekends) trying to understand why one of these services is bringing the whole system down.

Without having the right measurements in place you will never know if a change besides adding a feature, is slowing down your entire customer experience or if it is going to cause some downtime in the future.

It is also important to understand that for your users it doesn’t really matter if you are using Kubernetes, Containers or even building Cloud-Native applications, all they look for are new and better applications that help them to solve their problems. To make sure that value to the user is always our true north, we rely on Continuous Delivery practices. Continuous Delivery (CD for short), as presented in the Continuous Delivery Book (a reference to be added), is a set of practices that helps teams to deliver valuable software to their users in a reliable and efficient way. CD focuses heavily on automation and measuring our delivery processes, to always keep improving. Explaining all of the practices involved in CD is beyond what we want to cover in this chapter; for now, I’ll simply explain the goals behind CD and why you should care about learning and apply CD to your day to day software practice. In the following section, we will focus on understanding in more detail Continuous Delivery goals and how they relate to Cloud-Native applications. We will go over the practices in the following chapters when we deep dive into some of the tools that were created to help us apply these practices to our projects.

# 1.2 Continuous Delivery Goals

Delivering valuable software to your customers/users in an efficient way should be your main goal. While building Cloud-Native applications, these become challenging as you are not dealing with a single application, you are now dealing with complex distributed applications and multiple teams delivering features at different paces.

For the remainder of the book, the following goal of Continuous Delivery is going to be used, to guide the selection of different projects and tools for your teams to use:

From the Book Continuous Delivery

> "Goal: Deliver useful, working software to users as quickly as possible
> Focus on Reducing cycle time (The time from deciding to make a change to having it available to users)"

This goal definition come from the Continuous Delivery book wrote by Jez Humble and David Farley (2010 https://www.amazon.co.uk/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=asc_df_0321601912/?tag=googshopuk-21&linkCode=df0&hvadid=3109134879798&hvpos=&hvnetw=g&hvrand=15021799242967420863&hvpone=&hvptwo=&hvqmt=&hvdev=c&hvdvcmdl=&hvlocint=&hvlocphy=9045944&hvtargid=pla-4345140654988&psc=1&th=1&psc=1). The book, on purpose, doesn’t go deep into any technologies besides naming them, and because it was written more than 10 years ago, the Cloud and Kubernetes didn’t exist in the way that exists today.

There are some significant areas covered by Humble and Farley’s book which you will read about here, such as: