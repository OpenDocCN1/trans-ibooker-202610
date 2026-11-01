# GitHub and the git push Command

Although Git repos can exist entirely on your computer, many free websites can host clones of your repo online, letting others easily download and contribute to your projects. The largest of these sites is GitHub. If you keep a clone of your project online, others can add to your code, even if the computer from which you develop is turned off. The clone also acts as an effective backup.

> NOTE
> 
> Although the terms can cause confusion, Git is version control software that maintains a repo and includes the git command, whereas GitHub is a website that hosts Git repos online.

Go to https://github.com and sign up for a free account. From the GitHub home page or your profile page’s Repositories tab, click the New button to start a new project. Enter wizcoin for the repository name and the same project description that we gave Cookiecutter in “Using Cookiecutter to Create New Python Projects” on page 200, as shown in Figure 12-6. Mark the repo as Public and deselect the Initialize this repository with a README checkbox, because we’ll import an existing repository. Then click Create repository. These steps are effectively like running git init on the GitHub website.

![](bbox=[0.24, 0.47, 0.89, 0.81])

Figure 12-6: Creating a new repo on GitHub

You’ll find the web page for your repos at https://github.com/<username>/<repo_name>. In my case, my wizcoin repo is hosted at https://github.com/asweigart/wizcoin.