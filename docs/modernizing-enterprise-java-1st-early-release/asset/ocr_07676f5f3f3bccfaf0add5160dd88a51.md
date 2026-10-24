applicable example to share as it heavily depends on the application itself, the business requirements, and the overarching company goals and challenges. Document your decisions and conclusions because now is the time to decide about the future direction of your modernization effort. Remember the 6 R’s from chapter three? Retain (change nothing), Retire (turn off), Repurchase (a new version), Rehost (put into containers), Replatform (some slight adjustments) or Refactor (Build something new).

We’ve now assessed the application for migration and we’ve evaluated the functionality for migration. We know which aspects of the application we’re ready to modernize. You’ve concluded that you do not want to build a new application but rather gently modernize the existing legacy. In the next section, we are going to take a deeper look at some approaches to migration.

# Migration Approaches

The aforementioned tools and assessments will help you on your journey to identify the most suitable applications and services. Now it’s time to dig deeper into the strategies and challenges of a single application.

# Protecting Legacy (Replatform)

With only one or two modules needing a business refresh or added functionality, the most straightforward way is to focus on the two modules and keep as much as possible of the existing application, making it runnable on modern infrastructure. Besides changes of the relevant modules, this also involves a re-evaluation of the runtime, libraries or even target infrastructure while touching as little code as possible.

This can be achieved by simply containerizing the application and databases and modifying relevant modules of a well-architected monolith, or extracting certain functionality completely and re-integrating it to a partly distributed system, as Figure 5-2 shows.