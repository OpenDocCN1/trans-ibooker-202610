That said, there is one quick way to add a lot of detail to your traces, which is to convert any existing logs into trace events. This can be done by creating a simple log appender that grabs the current span and attaches the log to it as an event. All of your application logs are now available when you look up the trace, which is far easier than finding them in a traditional logging tool. OpenTelemetry does provide log appenders for some common logging systems, but they are also easy to write.

# Work with Management

If you’re an engineer reading this, I have one additional note. Rolling out a new telemetry system can involve organizing a lot of people. Luckily, there are people who already do that kind of organizing—the managers!

I know, I know—managers. But if you’d like to start one of these initiatives, convincing an engineering or project manager to help is a great first step. They will have valuable insight into how to get it done and will be able to pitch the project in meetings you may not attend. Sometimes organizing people is harder than organizing code, so don’t be afraid to ask for help!

# Join the Community

Lastly, both on a personal and organizational level, consider joining the OpenTelemetry community! Maintainers and project leaders are friendly and very accessible. The community is an excellent resource for assistance and expertise; we are always happy to help new users get oriented. There is also a sweat equity culture: if there are OpenTelemetry features you’d like to see added, joining a working group and helping out is a great way to get them prioritized!

At minimum, be sure to give us feedback. The more we hear from users, the better we can focus on the issues that matter most. Our goal is to build the standard that drives the next generation of observability. We can’t do it without you, and our door is always open.