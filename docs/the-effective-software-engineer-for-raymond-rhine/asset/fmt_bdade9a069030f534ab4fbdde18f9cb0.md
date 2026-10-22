
These kinds of communication tasks often differentiate effective engineers from merely efficient ones. By ensuring everything around the code (the context in which the code lives) is updated and broadcast, Sam prevents a lot of future pain—fewer confused users, fewer on-call surprises, more transparency. AI here acts as a force multiplier for thoroughness. It’s easier to be diligent when the busy work of diligence (writing docs, summarizing changes) is alleviated by an assistant. As a bonus, this ties back to advice in [Chapter 4](#ch04.html#ch04_collaboration_and_cross_functional_influence_1768431688998639) about keeping stakeholders informed and in [Chapter 2](#ch02.html#ch02_understanding_the_fundamentals_junior_to_mid_leve_1768431688783317) about writing good documentation; AI helps you do those soft tasks more effectively, not just the hardcore coding.

At this point, Sam’s day (or sprint) is complete: from planning to rollout, AI has been a helpful collaborator, and every step’s output feeds the next. The planning outputs created the to-do list for coding; the tests made coding safer; the code and tests made review easier; the code and monitoring changes made documentation and comms straightforward. For an IC, AI shines through the steady accumulation of small assists, creating a compounding effect that allows the team to move significantly faster and with greater confidence.

## Fictional Example: Sam’s AI-Powered Workday

Let’s take a look a how all these pieces come together in a single (very productive) workday for Sam:

9:00 a.m.—planning

Sam starts the day with a feature idea and a vague requirement doc. By 9:30, with the AI’s help, Sam has a one-page decision brief comparing four implementation approaches, complete with trade-offs and links to case studies. At 10:00, Sam uses the AI to poke holes in the preferred approach’s scalability and failure modes, uncovering a potential performance bottleneck that wasn’t obvious at first. By 10:30, Sam has drafted an architecture decision record that the team can review during the 11:00 design meeting.

11:00 a.m.—design review

In the team meeting, Sam presents the AI-assisted ADR draft. The team discusses it, and since the draft already included multiple options and risk analysis, they quickly converge on a decision, with minor tweaks. The meeting ends early. Sam takes 15 minutes to update the ADR with a couple of manual edits based on the feedback, then finalizes it.

12:00 p.m.—breaking down tasks

Right before lunch, Sam feeds the project PRD into the AI and gets a structured backlog out. After reviewing and adjusting it, Sam now has a full list of epics and stories in the tracker. The project suddenly feels organized and manageable, rather than a big fuzzy blob of work.

1:30 p.m.—coding (tests first)

After lunch, Sam picks the first story (backend API for image upload). Using AI, Sam generates a suite of unit tests describing the expected behavior. By 2:00, Sam is iterating on implementation. The first AI-suggested code fails one of the tests (it didn’t handle a certain file type correctly), so Sam refines it, and soon all tests pass. Sam commits this chunk. The next story (frontend form) is tackled similarly: by 3:00, tests are written (the AI helped), code is implemented (the AI suggested most of it; Sam tweaked), and all tests are green. Sam commits again.

3:30 p.m.—self-code review

Before pushing the code for review, Sam uses the AI to double-check his own diff. The AI flags a possible improvement: “Hey, you’re logging the image size in two different places, maybe consolidate or ensure the format is consistent.” Sam realizes he did add redundant logging—a quick refactor fixes it. The AI also suggests adding a test for an empty image file scenario. Sam adds that test (it fails), then adjusts the code to handle it. By 4:00, the branch is in even better shape. Sam pushes the code.

4:15 p.m.—code review (peer and AI)

Jen, the reviewer, gets a ping. She opens the PR and runs her AI assistant on it to generate a review plan. By 4:30, Jen has left a few comments—two of which were formulated by AI (style nits), and one she wrote herself about a naming clarification. Sam quickly responds (the AI even helps draft a clarification comment for one of Jen’s questions). By 4:45, Jen is satisfied and approves the PR.

5:00 p.m.—documentation and wrap-up

The code is merged. Sam spends the last part of the day on polish: using AI to update the user guide section for profile pictures, adding a note in the team’s on-call playbook about the new service (“If the profile-pic service is down, users will see a default avatar”), and drafting a summary for the release notes and an internal email. All of these take maybe 15 to 20 minutes, with the AI doing the heavy lifting. Sam skims them, makes small edits, and sends them out.

5:30 p.m.—reflection

Sam looks back on the day: a design is decided on, tasks are defined, and code is written, tested, reviewed, merged, documented, and communicated—all in one day! It’s a lot of forward progress, with high quality. Sam didn’t skip tests or docs (common shortcuts under time pressure) because AI made those tasks almost effortless. There’s also a sense of confidence knowing that tests cover the feature and monitoring is in place. Sam heads off knowing that if anything goes wrong with this deployment, the team is prepared—and if things go right, users will immediately see value, and everyone knows about it.

This fictional day might sound idealized, but it’s increasingly attainable with the prudent use of AI tools. The point is not that Sam is some 10× hero (the tools did a lot of the grinding work)—the point is Sam invested time in leveraging AI to do things the right way (tests, docs, etc.), and as a result, both speed and quality were maximized. The workflow compounds: each step’s output makes the next step easier, creating a positive feedback loop that boosts overall effectiveness.

Having seen this soup-to-nuts example, we can move on to more specific tips. The next sections will cover crucial considerations like providing the AI with the right context, baking in security from the start, handling legal/privacy concerns, using autonomous AI agents carefully, measuring the impact of all this work, and even some ready-made prompt templates you can use. These will enrich the basic workflow we’ve outlined with additional best practices and safeguards.

# Feeding the AI Context: Getting Repository Awareness Right

One of the most important factors in getting useful, correct outputs from an AI assistant is *context*. An AI model is only as good as the information you give it (plus what it already “knows” from training). If you ask a generic AI about your specific codebase without providing context, you’ll get generic answers. Effective engineers take control of the context fed to the AI.

Here are some ways to give your AI buddy the awareness it needs:

Connect it to your codebase and knowledge sources.

The best AI coding tools at present integrate deeply with your development environment. This means the AI can “see” your repository (at least the parts of it relevant to your prompt), your API schemas, your database schemas, your service catalog, etc. If your tool supports it, ensure it’s plugged into your IDE with repository indexing enabled. If not, you can manually copy-paste relevant code snippets or definitions into the prompt. For example, if you want the AI to help with a function in a particular file, it helps to also provide the content of said file (or at least the signature of the function and related data structures) in the prompt. The difference is night and day—an assistant with context can tailor suggestions to your actual code, whereas one without is guessing from general knowledge.

Use semantic search for deep references.
