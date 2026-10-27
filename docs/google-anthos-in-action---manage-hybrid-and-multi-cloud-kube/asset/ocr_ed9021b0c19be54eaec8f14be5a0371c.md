instead of “hello world” Skaffold will automatically detect the change, rebuild the image, push it to the registry and deploy. As the logs tail to the console, you should see the message “hello from Skaffold”.

# Single run with Skaffold

While Skaffold dev has been continuously watching the source files, you can also perform a single execution of the workflow by running: skaffold run command. This is useful when you want to run the execution only once and not to trigger the flow every time your code is modified.

# Supported features

By now we should have a basic understanding of how to start development with Skaffold so let's have a look at other useful features available.

# Pipelines stages

Till now, we have looked only at the basic functionalities of Skaffold, however the tool has more capabilities that can address advanced pipeline stages. As an example a developer might not want to rebuild the images after every code change. In such a case Skaffold can synchronize the files into the container main.go source file. To achieve this, you would use the File Sync feature. Below you can find a list of all the currently available stages:

Figure 14.5 Skaffold workflow

![](bbox=[0.11, 0.71, 0.88, 0.89])