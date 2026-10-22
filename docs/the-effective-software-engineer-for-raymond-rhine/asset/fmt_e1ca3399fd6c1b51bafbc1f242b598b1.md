
# Leading a Cross-Team Infrastructure Overhaul

Let’s look at the following scenario: the company needs to overhaul its infrastructure (say move from on-prem to cloud). It’s a multi-team effort. They appoint you, a staff engineer, as the tech lead architect for it. You have no direct reports, but you must align five teams and 50 engineers toward this common goal over six months.

Here’s how you can succeed:

## Form a working group

Bring representatives (maybe the most senior or knowledgeable ICs) from each team into a working group. Establish clear roles—maybe one person coordinates networking changes, another databases, etc., under the overall plan. Now you have point people to collaborate with instead of dealing with 50 individuals. This group will be your go-to for disseminating info and gathering status. It’s classic “matrix” leadership: you lead these people regarding the initiative, even though they report elsewhere.

## Develop and communicate a plan

Create the technical vision (target architecture) and a phased migration plan. Communicate this widely: present at engineering all-hands, write an arch doc for Confluence, etc. People need to see the whole map, not just their piece, so they understand how their work ties into the larger project. Use visual diagrams and timeline charts if it helps. As the leader, you’ll maintain and update this plan as things evolve.

## Project management (yes, you do some PM work)

Identify dependencies between teams and make them explicit. For example, Team A must deliver a new API by February for Team B to switch in March. Track progress (using spreadsheets or a project management tool). Follow up in sync meetings. Senior ICs often partner with an engineering manager or technical program manager (TPM) on this, but sometimes you have to do a lot of it. The key is ensuring no team is blocked, or if they are, it’s surfaced and resolved.

## Technical oversight

While you can’t code everything, you should review key designs from each team for alignment. Perhaps set up a weekly migration design review where each team presents what they’re doing next and any challenges, and as the initiative lead, you ensure it all fits together or give advice if one team is approaching something suboptimally. You don’t micromanage (teams need autonomy), but you gently guide. Think consultant more than boss.

## Unified standards and tools

For a big initiative, having common approaches accelerates progress. If each team writes its own migration scripts differently, it’s chaos. You, as leader, might propose, “Let’s all use X tool for infrastructure-as-code.” Provide a template or example others can copy. This reduces duplicate work and fosters shared expertise (everyone can help each other because approaches are similar). It also cements your leadership role by providing helpful resources.

## Cross-team communication

Ensure wins and discoveries are shared across teams. Maybe set up a Slack channel (say, “#cloud-migration”) for any gotchas found, so others don’t duplicate debugging. Share frequent updates on what’s done so far, and make sure to celebrate milestones publicly. This not only motivates (progress is visible) but also keeps everyone informed. Often, you become the voice of the project in broader forums: giving status to directors, highlighting risks, etc. That is leadership: representing an effort and driving it to success.

## Handle conflict or misalignment diplomatically

If Team X is behind or Team Y built something not per the agreed specifications, approach it via problem solving, not blame. “I see we’re behind here—how can we adjust? Do you need help or can another milestone move?” or “This component ended up different from what API spec said; let’s reconcile—maybe the spec was incomplete? Let’s figure it out.” Large projects have issues; the leader sets a collaborative tone for fixing them rather than finger-pointing. This keeps morale and trust high, which is crucial over long efforts.

## Give credit, take responsibility

When reporting on the successes, acknowledge the teams: “Team A and B did an awesome job hitting their targets.” When reporting problems, own them collectively or as a leader: “We underestimated the effort here—that’s on me; we have a plan to get back on track.” This protective stance endears you to teams (they know you won’t throw them under the bus) and impresses higher-ups (you show leadership maturity).

Leading big initiatives as an IC is challenging but can be a pinnacle of IC career achievement. You orchestrate a symphony of efforts, making sure the final outcome is harmonious. Many staff/principal engineers find this aspect—making something big happen across an org—immensely rewarding, rivaling or exceeding the gratification from writing code individually.

Successfully driving a big cross-team project is often what distinguishes a staff engineer from a senior engineer in promotion calibrations. It shows you can operate at an org level. It’s also, not incidentally, a skill that transfers to management roles if you ever choose that route.

This is engineering leadership in the pure IC form.

# Mentorship and Sponsorship: Lift Others as You Climb

I’ve mentioned mentorship several times. By senior levels, not only should you be mentoring others, but intentionally sponsoring promising talent too. This is a form of leadership that ensures the next generation grows, and it’s often expected at high IC levels (many career ladders have criteria like “actively mentors others” for seniors and “develops other technical leaders” for principals). As a reminder:

* A *mentor* gives advice, answers questions, and provides feedback to help someone grow.
* A *sponsor* uses their influence to advocate for someone, recommending them for a key project or promotion.

Here are a few ideas for how to mentor effectively as a senior IC:

* Take on at least one regular mentee (often a new hire or junior). Set up a weekly or biweekly chat. Help them solve problems rather than giving direct answers. Ask guiding questions, share similar experiences.
* In code reviews, don’t just fix issues, explain patterns and reasoning. For example, “I refactored this because our convention is X, which helps because Y. Keep it in mind next time.”
* Encourage them to take on some stretch tasks and be their safety net. “Why don’t you lead the implementation of this feature? I’ll support you and review your design.” They learn by doing, but know you have their back. This is huge for building their confidence and skills.
* Protect time for mentorship despite being busy. It’s easy to deprioritize because it isn’t “urgent.” But remember, part of your job at senior levels is team growth (usually reflected in performance reviews).
* You also learn by mentoring; you solidify your own knowledge and gain perspective on how others approach things.

And you can take this a bit further with sponsorship as an IC leader:

* Identify junior/mid-level engineers who have potential. If you see someone doing great work quietly, draw attention to it. For example, tell your manager “Hey, I think Maria is doing senior-level work on that module—maybe consider her for promotion if she keeps it up.” Sometimes managers aren’t in the weeds to see it; your voice can highlight deserving folks.
* Nominate others for opportunities. If a new project needs a lead and you could do it but so could your colleague who’s trying to grow, support them taking the lead (with you advising). Leadership isn’t hoarding the best tasks; it’s growing more leaders.
* Invite a less-experienced teammate to co-present with you at an internal tech talk, giving them visibility.
* If you have a seat at calibration meetings (some companies involve staff engineers in promotion committees), speak up for those you think meet the bar.
