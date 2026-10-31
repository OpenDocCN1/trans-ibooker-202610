
With a monorepo approach, code for multiple microservices (or other types of projects) are stored in the same source code repository. I have seen situations where a monorepo is used just by one team to manage source control for all their services, although the concept has been popularized by some very large tech companies where multiple teams and hundreds if not thousands of developers can all work on the same source code repository.

By having all the source code in the same repository, you allow for source code changes to be made across multiple projects in an atomic fashion, and also allow for finer-grained reuse of code from one project to the next. Google is probably the best known example of a company using a monorepo approach, although it’s far from the only one. Although there are some other benefits with this approach - such as improved visibility of other people’s code for example - the ability to reuse code easily and to make changes that impact multiple different projects is often cited as the major reason for adopting this pattern.

If we take the example from above, where we want to make a change to the `Inventory` so that it exposes some new behavior, and also update the `Shipping` service to make use of this new functionality that we’ve exposed, then these changes can be made in a single commit, as we see in [Figure 6-9](#ch06.xhtml#atomic-commit-across-microservices).

![](Images/atomic-commit-across-microservices.png)

Of course, as with the multi-repo pattern discussed above, we still need to deal with the deployment side of this. We’d likely need to carefully consider the order of deployment if we want to avoid a lock-step deployment.

# Atomic Commits vs Atomic Deploy

Being able to make an atomic commit across multiple services doesn’t give you atomic rollout. If you find yourself wanting to change code across multiple services at once, and roll it out into production all at the same time, this violates the core principle of Independent Deployability. For more on this see [“DRY and the Perils of Code Reuse in a Microservice World”](#ch04.xhtml#a50-dry).

## Mapping To Build

With a single source code repository per microservice, mapping from the source code to a build process is straightforward. Any change in that source code repository can trigger a matching CI build. With a monorepo, it gets a bit more complex.

A simple starting point is to map folders inside the monorepo to a build, as shown in [Figure 6-10](#ch06.xhtml#a61-single-repo-multi-builds). A change made to the `user-service` folder would trigger the `User service build` for example. If you checked in code that changed both files in the `user-service` folder and the `catalog-service` folder, then both the `User service build` and the `Catalog service build` would get triggered.

![](Images/monorepo.png)

This gets more complex as you have more involved folder structures. On larger projects you can end up with multiple different folders wanting to trigger the same build, and some folders triggering more than one build. At the simple end of the spectrum you might have a `common` folder used by all microservices, a change to which causes all microservices to be rebuilt. At the more complex end, teams end up needing to adopt more graph-based build tools like the open source `Bazel` tool to manage these dependencies more effectively (`Bazel` is an open source version of Google’s own internal build tool). Having to implement a new build system can be a significant undertaking, so is not something to be done lightly - but Google’s own monorepo would be impossible without tools like this.

One of the benefits of a monorepo approach is that you can practice finer-grained reuse across projects. With a multi-repo model, if I want to reuse someone else’s code, it will likely have to be packaged as a versioned artifact that I can then include as part of my build (such as a nuget package, JAR file, or NPM). With our unit of reuse being a library, we are potentially pulling in more code than we really want. Theoretically, with a monorepo I could just depend on a single source file from another project - although this of course will cause you to have a more complex build mapping.

## Defining Ownership

With smaller team sizes, and small codebase sizes, monorepos can likely work well with traditional build and source code management tools that you are used to. However, as your monorepo gets bigger, you’ll likely need to start looking at different types of tools. We’ll explore ownership models in more detail in [Link to Come], but in the meantime it’s worth exploring briefly how this plays out when we think about source control.

Martin Fowler has previously written about different ownership models before^([6](#ch06.xhtml#idm46534869957464)), outlining sliding scale of ownership from `Strong Ownership`, through `Weak Ownership`, and on to `Collective Ownership`. Since Martin captured those terms, development practices have changed, so it’s perhaps worth revisiting and redefining these terms.

With `Strong Ownership`, some code is specifically owned by a group of people. If someone from outside that group wants to make a change, they have to ask the owners to make that change for them. `Weak Ownership` still has the concept of defined owners, but people outside of this ownership group are allowed to make changes, although any of these changes must be reviewed and accepted by one of the ownership group. This would cover the use of pull requests being sent to the core ownership team for review, before the pull request is merged. With `Collective Ownership`, any developer can change any piece of code.

With a small number of developers (20 or less, as a general guide), you can afford to practice `Collective Ownership` - where any developer can change any other microservice. As you have more people though, you’re more likely to want to move towards either `Strong` or `Weak` ownership model to create more defined boundaries of responsibility. This can cause a challenge for teams using monorepos if their source control tool doesn’t support finer-grained ownership controls.

Some source code tools allow you to specify ownership of specific directories or even specific file paths inside a single repository. Google initially implemented this system on top of Perforce for their own monorepo, before developing their own source control system, and it’s also something that GitHub has supported since 2016^([7](#ch06.xhtml#idm46534869951576)). With GitHub, you create a `CODEOWNERS` file, which lets you map owners to directory or file paths. You can see some examples in [Example 6-1](#ch06.xhtml#codeowners-file), drawn from GitHub’s own documentation, showing the kinds of flexibility these systems can bring.

### Example 6-1. Examples of how to specify ownership in specific directories in a GitHub CODEOWNERS file

```
# In this example, @doctocat owns any files in the build/logs
# directory at the root of the repository and any of its
# subdirectories.
/build/logs/ @doctocat

# In this example, @octocat owns any file in an apps directory
# anywhere in your repository.
apps/ @octocat

# In this example, @doctocat owns any file in the `/docs`
# directory in the root of your repository.
/docs/ @doctocat
```

GitHub’s own code ownership concept ensures that code owners for source files are requested for review when any pull request is raised for the relevant files. This could be a problem with larger pull requests as you could end up needing sign-off from multiple reviewers, but there are lots of good reasons to aim for smaller pull requests in any case.

## Tooling
