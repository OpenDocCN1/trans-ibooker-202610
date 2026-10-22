Figure 1-14. - The new example task automatically added into the Kanban board

# Source Code Control Using Git Remotes and Branching

One of the key benefits of using Git is the ability to collaborate with others and synchronize your local code repository with a remote repository stored in the cloud. If you’ve worked with Git before, you’re already familiar with remotes and branching. I’m not going to go into significant detail here, but they’re important concepts to grasp as part of your automation environment.

Branching is simply a way of enabling each developer, or team, to segment their work without negatively impacting the main code base. In Git, this default main branch is currently called ‘master’, and is typically used to contain the code that has been completed, tested, verified, and - usually - placed into production. This is the default branch for both ‘programmingtheiot-java’ and ‘programmingtheiot-python’, and while you can leave it as is and simply work off this default branch, it’s not generally recommended for the reasons I mentioned.

Branching strategies can differ from company to company and team to team, although the one I like to use has each chapter in a new branch, then once all is working correctly and properly tested, the chapter branch gets merged into the master. From there, a new branch is created from the merged master for the next chapter, and so on.