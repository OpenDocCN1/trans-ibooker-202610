
> Change summary: Added profile picture upload feature including backend API and frontend UI. Risky parts: handling of image input (security), new dependency on image library (performance). Suggested comments: \[Line 42\] ‘Consider limiting the file size to a configurable max to avoid large payloads.’

Use these suggestions as input—you (the human reviewer) make the final call on which comments to actually post, but this can catch things or help word them. It’s also a great way to prep for a review if you’re short on time: you get a quick brief of what to look at.

## Migration planner

When you need a step-by-step plan to migrate tech (libraries, services, etc.), use this prompt:

> We need to migrate from <Library/Tech A> to <Library/Tech B>. Draft a safe, step-by-step migration plan. Include: necessary code changes or rewrites, ways to run A and B in parallel during transition if possible, any data migration needs, how to verify everything works at each step, and a rollback plan in case issues occur. Emphasize minimizing downtime and risk. The output should be structured by phases (e.g., Phase 1: do X, Phase 2: do Y). Also note any gotchas or things to watch out for from similar migrations (you can infer common pitfalls).

The AI, if it has knowledge of both technologies, will outline something like this:

> Phase 1: update dependency and set config to use new library in shadow mode, Phase 2: run both libraries side by side… Phase 3: switch traffic… Phase 4: remove old library.

It might mention telemetry (“monitor memory usage, since lib B might use more RAM”). This is great for ensuring you don’t overlook a step. Of course, verify specifics, but it can serve as a first draft of a migration runbook. If external references are needed (maybe known issues migrating from X to Y), you could also prompt it to list references, which you then check.

## Incident explainer

Using AI to help with post-mortems and blameless analysis would look like this:

> Here is a timeline of an incident and some log excerpts: <provide incident data>. You are an incident analysis assistant. Please:
>
> (a) Write a concise narrative of what happened during this incident in plain English, as you would in a post-mortem report.
>
> (b) Identify the key contributing factors or root causes that led to the incident (list two to three).
>
> (c) Propose two actionable, blameless remediation steps that would help prevent similar incidents in the future. These should be realistic and focus on system or process improvements (not pointing at individuals).

This prompt can take quite a bit of context (timeline of events, error messages). The AI will output a draft incident report. For example:

> **Incident Summary:** On 2025-05-01 at 09:00 UTC, service X experienced a 15-minute outage due to a memory leak that exhausted the container memory. The leak was introduced by a recent deployment…
>
> **Contributing Factors:** 1) Lack of memory usage alerts on service X. 2) The new image processing module wasn’t covered by load testing…
>
> **Remediations:** Implement memory usage monitoring and alerting for service X. Add a load test for image processing to the CI pipeline to catch leaks before deployment.

This can save a lot of time in writing post-mortems and also ensure you cover the bases (timeline, causes, actions). Of course, you’ll fact-check it against what you know and adjust wording, but it provides a solid starting structure.

These patterns have worked in the sense that they produce useful, actionable outputs that often only need minor tweaks. They encapsulate a lot of the advice from [“Measuring Impact: How to Quantify AI’s Effect Like an Engineer”](#ch13.html#ch13_measuring_impact_how_to_quantify_ai_s_effect_like_1768431690629963): they frame tasks clearly, include constraints (especially security), and request structured outputs. You can modify these to fit your project’s style (for instance, maybe you have an internal term for post-mortems, or a specific format for test names).

A good practice is to keep these patterns versioned. If you or a teammate refines a prompt and finds an even better way to ask it, update the template in your shared docs. Over time, you build an internal prompt cookbook that is tailored to your workflows, which is a real competitive advantage.

Also, remember that AI models change (a prompt that worked great on model version X might need tweaking on version Y), so occasionally revisit and test your prompt templates, especially after any major AI tool update. Encourage your team to contribute new patterns as they discover them. For example, someone might add “data analysis assistant”—a prompt to analyze a computerized system validation of metrics—if that proves handy.

These prompt patterns are like little power-ups. They embody effective prompting techniques and domain knowledge so you don’t have to start from scratch every time. Use them, share them, and adapt them—they’ll make your daily interactions with AI smoother and more productive.

# Rolling AI Out to Your Team: A 90-Day Plan

If you’re an IC, and not in a position to drive team-wide adoption, you can still introduce AI from the ground up. Start by using it yourself on low-risk tasks, document clear wins (time saved, bugs caught), and share those results informally with teammates. Offer to pair program with AI in team sessions to demonstrate its value. Once you’ve built credibility, propose a small pilot to your manager with specific metrics you’ll track. Even without formal authority, demonstrating tangible benefits can help you become a catalyst for broader adoption.

Adopting AI in a team or organization isn’t just flipping a switch. It’s a change in tools and culture. It helps to approach it deliberately. I’ll outline a pragmatic 90-day rollout plan for introducing AI assistance to a software team. This can be adapted based on team size or complexity, but it provides a rough timeline with milestones.

## Days 0–30: Establish Foundations and Guardrails

For the first month, focus on setup and understanding the baseline. That will look like the following:

### Pick your pilot tools and integration points.

There are many AI coding assistants out there (`Copilot`, `CodeWhisperer`, internal LLMs, etc.). Choose one to start with—ideally something that integrates with your team’s IDEs or workflow. Also ensure it can work with your codebase size/language. Keep it simple; you can expand later, but initially minimize variables (one tool, a couple of languages). Get the necessary licenses or approvals.

### Define data and security policies.

Before anyone starts using the AI, set the ground rules: “Don’t paste sensitive data into it,” “Don’t commit AI-generated code without review,” etc. Configure the tool’s settings for privacy (opt out of data sharing if possible, set up any available corporate controls like allowed repositories). Also figure out logging—e.g., will you capture all prompts? (You might not need to, but decide up front.) Basically, treat this like introducing any third-party software: do a mini threat model and compliance check. This might involve your security team, so start early.

### Instrument metrics and gather baseline information.

We discussed measuring in [“Quality and Throughput Metrics”](#ch13.html#ch13_quality_and_throughput_metrics_1768431690630068). In the first two to four weeks, measure how things are *without* much AI (or with just a couple of early adopters). For example, have a subset of tasks done the old way and time them, measure code quality outcomes, etc. Also, set up whatever telemetry you can get from the tool (how to measure suggestions, etc.). If the tool has an enterprise dashboard, explore it now. You want a baseline to compare against. If you have historical data (like average PR merge time last quarter), record that too.

### Begin with volunteers and small wins.
