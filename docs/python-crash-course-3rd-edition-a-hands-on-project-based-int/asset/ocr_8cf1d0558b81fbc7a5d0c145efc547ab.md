# ## Ongoing Development

You might want to further develop Learning Log after your initial push to a live server, or you might want to develop your own projects to deploy. When doing so, there’s a fairly consistent process for updating your projects.

First, you’ll make the necessary changes to your local project. If your changes result in any new files, add those files to the Git repository using the command `git add .` (making sure to include the dot at the end of the command). Any change that requires a database migration will need this command, because each migration generates a new migration file.

Second, commit the changes to your repository using `git commit -am "commit message"`. Then push your changes to Platform.sh, using the command `platform push`. Visit your live project and make sure the changes you expect to see have taken effect.

It’s easy to make mistakes during this process, so don’t be surprised when something goes wrong. If the code doesn’t work, review what you’ve done and try to spot the mistake. If you can’t find the mistake or you can’t figure out how to undo it, refer to the suggestions for getting help in Appendix C. Don’t be shy about asking for help: everyone else learned to build projects by asking the same questions you’re likely to ask, so someone will be happy to help you. Solving each problem that arises helps you steadily develop your skills until you’re building meaningful, reliable projects and answering other people’s questions as well.

# ## Deleting a Project on Platform.sh

It’s great practice to run through the deployment process a number of times with the same project or with a series of small projects, to get the hang of deployment. But you’ll