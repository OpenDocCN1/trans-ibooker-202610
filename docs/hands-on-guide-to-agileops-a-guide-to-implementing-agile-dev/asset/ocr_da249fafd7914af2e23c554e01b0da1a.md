**Note** Remember that every story should be independent, simple, and testable. Initially, teams may shy away from the estimation processes, but with the right training and mentorship, the teams will learn the methodology and realize the benefits. After all, this is needed for effective capacity planning.

# Defining Acceptance Criteria

Another important aspect of agile stories is to define the acceptance criteria. This means that if a user story is to be moved ahead or is considered complete, then it has to be thoroughly tested. This is made possible by defining acceptance criteria. This can happen either at the epic level or at the story level. In tools like JIRA, you can define fields that prompts the users/business analysts to mention this list of acceptance criteria. If the result for all the listed criteria is true, then this means that the story can move ahead. Testing should include functional as well as nonfunctional areas. The golden rule is that each story should be linked with at least one acceptance criteria. Each criterion is written in the format “given-when-then” that mentions the scenario and the expected result. It is good practice to build acceptance criteria with user stories as they are built. Table 8-4 shows a sample user story.

# Table 8-4. Sample User Story

| Story | Acceptance Criteria |
|---|---|
| As a: System administrator
I want to: Integrate tool 1 with tool 2 through a REST API module
So that: Alert notifications are visible and efficiently managed. | **Best Case:**
Given: Tool 1 is integrated with Tool 2.
When: A REST API is used.
Then: Notifications are visible to the system administrator.

**Worst Case:**
Given: Integrations for Tool 1 and Tool 2 are not active.
When: A REST API is deactivated/not working.
Then: The system administrator is updated about the disintegration.

**Nonfunctional Case:**
Given: Tool 1 and tool 2 are integrated.
When: A REST API is used.
Then: Notifications should be pushed every one minute. |