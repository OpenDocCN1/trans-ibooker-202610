# # Building an Angular application with the GitHub API

GitHub contains an API that we can use to fetch various information about the profile of a GitHub user. The Angular application that we are building will communicate with the GitHub API and display a brief portfolio for our GitHub profile. Our application will consist of the following features:

+   - **Dashboard:** This will be the landing page of the application, and it will display a summary of our GitHub profile.
+   - **Info:** This will display personal information about us.
+   - **Repositories:** This will display a list of our *public* repositories.
+   - **Organizations:** This will display a list of GitHub organizations of which we are members.

> **Important note**
> 
> The resulting output of each feature that is displayed in the screenshots of this chapter will be different according to your GitHub profile.

The dashboard will be the main page of the application, and it will contain all the other features. We will learn how to build the dashboard page in the following section.

# ## Building the dashboard

Before we can start creating the main features of our application, we need to scaffold and configure an Angular application first by running the following command:

```
```bash
ng new gh-portfolio --routing=false --style=scss
```
```

The preceding command will use the `ng new` command of the Angular CLI, passing the following options:

+   - `gh-portfolio`: The name of the Angular application that we want to create
+   - `--routing=false`: Disables routing because our application will consist of a single page
+   - `--style=scss`: Configures the Angular application to use the SCSS stylesheet format when working with CSS styles