# Figure 12.21 Creating a Jenkins pipeline to build the configuration server

Enter an item name
configserver
» Required field
Freestyle project
This is the central feature of Jenkins. Jenkins will build your project, combining any SCM with any build system, and this can be even used for something other than software build.
Maven project
Build a maven project. Jenkins takes advantage of your POM files and drastically reduces the configuration.
Pipeline
Orchestrates long-running activities that can span multiple build agents. Suitable for building pipelines (formerly known as workflows) and/or organizing complex activities that do not easily fit in free-style job type.
Multi-configuration project
Suitable for projects that need a large number of different configurations, such as testing on multiple platforms, platform-specific builds, etc.
OK
Folder
Select an item name
Select the Pipeline option

# Figure 12.22 Configuring the GitHub webhook and repository URL in the Jenkins pipeline, step 1

General
Build Triggers
Advanced Project Options
Pipeline
GitHub project
Project url https://github.com/lhuaylupo/manning-smia/
Project repository URL
Pipeline speed/durability override
Preserve stashes from completed builds
This project is parameterized
Throttle builds
Build Triggers
Build after other projects are built
Build periodically
GitHub Branches
GitHub Pull Requests
GitHub hook trigger for GITScm polling
GitHub hook trigger for GITScm polling