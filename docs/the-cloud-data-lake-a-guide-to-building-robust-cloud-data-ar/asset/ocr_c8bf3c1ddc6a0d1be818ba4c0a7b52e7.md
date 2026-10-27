# Design Considerations for Your Data Lake

> Have no fear of perfection—you will never reach it.
> —Salvador Dali

In Chapters 1 and 2, we got a 10,000-foot view of what cloud data lakes are and some widely used data lake architectures on the cloud. The information in the first two chapters gives you enough context to start architecting your cloud data lake design; you must be able to at least take a dry-erase marker and sketch a block diagram that represents the components of your cloud data lake architecture and how they interact.

In this chapter, we are going to dive into the details of the implementation of the cloud data lake architecture. As you will recall, the cloud data lake architecture consists of a diverse set of IaaS, PaaS, and SaaS products that are assembled into an end-to-end solution. Think of these individual services as Lego blocks and your solution as the structure you build with Lego pieces. You might end up building a fort or a dragon or a spaceship—the choices are limited only by your creativity (and business needs). However, there are a few basics you need to understand, which is what we are looking at in this chapter.

We will continue to use Klodars Corporation to illustrate some examples of the decision choices.

# Setting Up the Cloud Data Lake Infrastructure

Most cloud data lake architectures fall under one of two categories:

+   - You want to build your cloud data lake from scratch on the cloud. You don’t have a prior data lake or data warehouse implementation and are starting from a clean slate.