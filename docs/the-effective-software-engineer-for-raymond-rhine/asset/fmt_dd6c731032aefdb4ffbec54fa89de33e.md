
Adoption and behavior metrics tell you *how* the AI is being used by the team. Some useful metrics include the following:

## Daily/weekly active users

How many engineers are actually using the AI assistant regularly? If you rolled out licenses to 50 devs but only 5 are using it weekly, that’s a sign of either low usefulness or awareness. Track usage rates over time to see if adoption grows as people get comfortable. Maybe you set a goal like “80% of the team uses the assistant at least three times a week within two months of rollout” as a success criterion.

## Suggestion acceptance rate

Out of all the code or suggestions the AI offers, what percentage do engineers accept into the codebase? This can often be measured via the tool’s telemetry or even by scanning commit diffs for AI markers. Industry reports show an average of around [30% acceptance](https://oreil.ly/gXe-L) for tools like Copilot. If your acceptance is, say, 5%, maybe the prompts need improvement or folks are using it for the wrong tasks. If it’s 60%, that might actually be a red flag that people are overly trusting of it, unless it’s well justified. Keep an eye on this. Also, note acceptance might vary by scenario (maybe high in test files, low in critical code).

## Retention of AI-generated code

It’s one thing to accept a suggestion initially, but do you end up modifying or deleting it later? The study linked earlier found developers kept 88% of the AI-generated code they accepted with minimal changes. High retention means the suggestions were solid (or at least harmless). Low retention (like if half of it gets rewritten later) might mean the AI often gives a draft that humans then have to clean up—which could be fine if that’s expected, but it could also hint that the AI isn’t saving as much time as it could.

## Time to first suggestion

How quickly does the AI offer help when you start a coding task? If it takes five minutes of typing to get anything versus five seconds, that affects flow. Some tools measure the time from opening a file to the first suggestion accepted. Faster is generally better because it means AI is integrated into the flow and not causing delay.

## Context usage

How often does the AI utilize in-project context in its answers? For instance, does it cite or reference in-repo functions and APIs (if the tool is capable of that)? If you see that suggestions frequently mention your internal APIs correctly, that’s a good sign it’s well-tuned. If it’s always suggesting generic or wrong calls, the context integration might be failing. You could measure this qualitatively or via logs that show references.

## Quality and Throughput Metrics

Quality and throughput metrics measure the outcomes of using AI in terms of your team’s actual work output and quality. These can include the following:

## Task completion time

Take a set of tasks (maybe story points or specific types of tickets) and compare how long they take with AI versus without. For example, if implementing a standard CRUD (create, read, update, and delete) endpoint used to take on average eight hours, and now, with AI assistance, it takes five hours, that’s a tangible improvement. It’s good to measure a baseline before AI and then after adoption. Some controlled experiments (like the one cited in [“AI Agents: What They Can Do and How to Keep Them Tamed”](#ch13.html#ch13_ai_agents_what_they_can_do_and_how_to_keep_them_t_1768431690629908)) saw ~55% time reduction [on a specific task](https://oreil.ly/qLZSu), but your mileage will vary. Even a 10–20% improvement on average task time is huge over months.

## PR cycle time (time to merge)

This is how long a pull request stays open before being merged. If AI helps with things like writing tests and catching issues earlier, you might see PRs get approved and merged faster. Also, if AI helps reviewers, the review cycle could shorten. Keep an eye on whether PRs are moving through the pipeline more quickly or with fewer review iterations after AI introduction.

## Throughput (features/bugfixes per interval)

At a higher level, are you delivering more stuff? This can be tricky to measure (since complexity of work varies), but you could look at, say, the number of tasks or stories completed per sprint per developer pre- and post-AI. (Note: avoid relying solely on story points, as they can be inconsistent across teams and may not accurately reflect delivered value.) Or lines of code produced (not a perfect metric, but if dramatically more or less, it says something). One company’s [field study](https://oreil.ly/QgVXx) found an increase in pull requests per developer after adopting AI, implying more work got done in parallel.

## Code quality metrics

This is key—speed means nothing if quality drops. Track *defect density* (bugs per 1,000 lines of code, for example) for AI-assisted code versus non-AI code. Also track *escaped bugs*: issues that weren’t caught in testing and hit production. If AI is introducing subtle bugs, you might see a bump in production incidents in areas where it was heavily used. Conversely, maybe AI helps reduce bugs by catching things (e.g., if it writes tests that a human might have skipped, quality could improve). Data will tell. Microsoft’s internal data, for instance, indicated no quality degradation in many cases and even [improvements in build success rates](https://oreil.ly/QgVXx) with AI usage—but you need to verify in your context.

## Test coverage changes

If you use AI to write tests, are you seeing your code coverage numbers go up? You can measure code coverage on AI-generated commits versus others. If each AI-aided feature comes with, say, 20% more tests, that’s a positive quality indicator. On the flip side, ensure those tests are meaningful (we’ve all seen 100% coverage that still misses the point). Possibly measure the *mutation testing score* (how well tests catch injected faults) on AI-written tests versus human tests to gauge their effectiveness.

## Security incident rates

As a specific quality aspect, measure if the rate of security findings (from pen tests, bug bounties, automated scans) per line of code changes. If before AI you had X vulns per KLOC (thousands of source lines of code) and after AI it’s 2X, that’s a problem—maybe relating to vibe coding or insufficient guardrails. Ideally, with the security practices we outlined, this stays flat or even goes down (conceivably, if AI is always prompted for secure code, maybe it avoids some common mistakes humans make).

### Tip

While the metrics in this section focus on internal development, don’t neglect production and stability metrics. Recent [DORA reports from 2024–2025](https://dora.dev) have noted potential negative impacts on delivery stability with AI usage in some contexts, so it’s essential to track production health alongside development speed.

One practical way to gather a lot of this data is to instrument your development process. Some AI tools provide dashboards (e.g., GitHub’s metrics API can give suggestion acceptance stats). You might also extract info from your version control history (perhaps tagging commits that had AI involvement, then comparing metrics).

Now, measuring is great, but we have to be careful to interpret and act on the data correctly. This is where you put on your experiment-designer hat. Consider running a structured pilot experiment when rolling out AI. For example:

## Baseline period

For the first couple of weeks, have a small team work *without* the AI on some representative tasks, but measure all the preceding metrics. This is your baseline performance without the new tool (this might be current state if you haven’t used it before).

## Introduction with control group
