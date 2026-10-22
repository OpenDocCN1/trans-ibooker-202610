
instrumented to help understand build and deployment performance. A big performance boost here will be felt across the organization and can make a good case for moving OpenTelemetry into production.

## Centralize Telemetry Management

Rolling out and managing a telemetry system benefits greatly from centralization. In some organizations, there is a platform or infrastructure team that has access to every service. A team such as this is a great place to centralize the management of telemetry, which can help immensely. The telemetry pipeline is best thought of as its own system; allowing one team to operate the entire telemetry pipeline is often better than asking many teams to each own a portion of the system.

On the software level, integrating OpenTelemetry setup with code management tools that are already widely deployed—shared init scripts and application frameworks, for example—reduces the amount of code each team needs to manage. This helps to ensure that services stay up-to-date with the latest versions and configurations and makes adoption much easier.

Another critical tool is a centralized repository of knowledge. OpenTelemetry has documentation, but it is generalized. Create documentation specific to deploying, managing, and using OpenTelemetry within your organization. It’s hard to overstress how much this helps, as most engineers will be new to OpenTelemetry and distributed tracing in general.

## Breadth Before Depth

One more note on spending your effort wisely. When instrumenting a workflow end to end, it is usually sufficient to install the instrumentation that comes with OpenTelemetry, plus instrumentation for any internal or homegrown frameworks used by your organization. It is not necessary to deeply instrument the application code, at least not at first. The standard spans and metrics that come with OpenTelemetry get you far enough to identify most problems; deeper instrumentation can be added selectively when necessary. Make sure you get end-to-end tracing up and running before adding this kind of detail.

That said, there is one quick way to add a lot of detail to your traces, which is to convert any existing logs into trace events. This can be done by creating a simple log appender that grabs the current span and attaches the log to it as an event. All of your application logs are now available when you look up the trace, which is far easier than finding them in a traditional logging tool. OpenTelemetry does provide log appenders for some common logging systems, but they are also easy to write.

## Work with Management

If you’re an engineer reading this, I have one additional note. Rolling out a new telemetry system can involve organizing a lot of people. Luckily, there are people who already do that kind of organizing—the managers!

I know, I know—managers. But if you’d like to start one of these initiatives, convincing an engineering or project manager to help is a great first step. They will have valuable insight into how to get it done and will be able to pitch the project in meetings you may not attend. Sometimes organizing people is harder than organizing code, so don’t be afraid to ask for help!

## Join the Community

Lastly, both on a personal and organizational level, consider joining the OpenTelemetry community! Maintainers and project leaders are friendly and very accessible. The community is an excellent resource for assistance and expertise; we are always happy to help new users get oriented. There is also a sweat equity culture: if there are OpenTelemetry features you’d like to see added, joining a working group and helping out is a great way to get them prioritized!

At minimum, be sure to give us feedback. The more we hear from users, the better we can focus on the issues that matter most. Our goal is to build the standard that drives the next generation of observability. We can’t do it without you, and our door is always open.

## Thanks for Reading

I hope you’ve enjoyed this report on the future of observability with OpenTelemetry. If you have any questions, comments, feedback, or inspirations based on what you’ve read, please feel free to reach out to me on Twitter—I’m @tedsuo.

For help getting started with OpenTelemetry, please join the #OpenTelemetry channel on the Cloud Native Computing Foundation’s (CNCF) Slack instance. I hope to see you there!

## OpenTelemetry Project Organization

OpenTelemetry is a large project. Work on the OpenTelemetry project is divided into *special interest groups* (SIGs). While all project decisions are ultimately made via GitHub issues and pull requests, SIG members frequently stay in touch through the CNCF’s official Slack instance, and most SIGs meet once a week on Zoom.

Anyone can join a SIG. To find out more details about current SIGs, project membership, and project bylaws, check out the OpenTelemetry community repo on GitHub.

## The Specification

OpenTelemetry is a specification-driven project. The OpenTelemetry Technical Committee maintains the specification and guides the project forward by managing the specification backlog.

Small changes may be raised as GitHub issues, with the subsequent pull requests submitted directly to the specification. But significant changes to the specification occur via a request for comment process called OpenTelemetry Enhancement Proposals (OTEPs).

Anyone can submit an OTEP. The OTEPs are reviewed by specification approvers, who are grouped according to their area of expertise and assigned by the Technical Committee. OTEPs require at least four approvals to be accepted. A detailed design, along with prototypes in at least two languages, is usually required before any approvals can be made. OTEP authors are expected to take the requirements and concerns of other community members seriously. The goal is to ensure that OpenTelemetry fits the needs of as broad an audience as possible.

Once accepted, spec changes are drafted based on the OTEP. Spec changes only require two approvals, as most issues have already been resolved during the OTEP process.

## Project Governance

The rules and organizational structures, which govern how the OpenTelemetry project functions, are defined and maintained by the OpenTelemetry Governance Committee, members of which are elected for two-year terms.

Governance members are expected to participate as individuals, not company representatives. But there is an upper limit to the number of committee members who may work for the same employer. If that threshold is exceeded because a committee member has changed jobs, committee members must resign until employer representation drops below the threshold.

## Distros

OpenTelemetry has a plug-in-based architecture, since some observability systems require a set of plug-ins and configurations to function correctly.

*Distros* (distributions) are defined as a collection of widely available OpenTelemetry plug-ins, plus a set of scripts or helper functions that may make it simpler to connect OpenTelemetry to a particular backend or run OpenTelemetry in a particular environment.

It is important to clarify that, for an observability system to claim it is compatible with OpenTelemetry, it should always be possible to use OpenTelemetry without being required to use a particular distro. If a project extends the core functionality of OpenTelemetry without going through the specification process, or includes any changes that cause it to become incompatible with the upstream OpenTelemetry repos, then that project is a fork, not a distro.

## The Registry

To make it easy to discover what languages, plug-ins, and instrumentation are currently available, OpenTelemetry provides a registry. Anyone can submit a plug-in to the OpenTelemetry registry; hosting the plug-in within the OpenTelemetry GitHub organization is not a requirement.

## OpenTelemetry Project Roadmap

Roadmaps go out-of-date quickly. For the most up-to-date roadmap, please see the OpenTelemetry status page. That said, here’s the state of OpenTelemetry as of this writing.

## Core Components

Currently, the OpenTelemetry Tracing Signal is declared stable, and stable implementations are available in many languages.

The Metrics Signal has just been declared stable, and beta implementations will be widely available in Q1 of 2022.

The Logging Signal is expected to be declared stable in Q1 of 2022, with beta implementations expected to be widely available in Q2 of 2022. In 2021, the Stanza project was donated to OpenTelemetry, adding highly efficient log-processing capabilities to the OpenTelemetry Collector.

The Semantic Conventions for HTTP/RPC, databases, and messaging systems are expected to be declared stable in Q1 of 2022.

## The Future

Finishing the above roadmap completes the stability requirements for OpenTelemetry’s core functionality. This is a huge milestone, as it opens the door to further adoption of OpenTelemetry, including native integration with databases, managed services, and OSS libraries.
