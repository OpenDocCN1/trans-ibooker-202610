
## Add Git Repositories

# Search and select Git repositories on your local file system

Found one repository scanning one folder in 1ms

Search criteria

Directory: E:\aking\programmingtheiot\java-components

Browse...

Search

Look for nested repositories

Skip hidden directories

Search results

type filter text

E:\aking\programmingtheiot\java-components\.git

Finish

Cancel

Figure 1-5. - Import java-components from your local Git repository

Click ‘Finish’, and you’ll see your new repository added to the list of repositories you can import. Highlight this new repository and click ‘Next…’. Eclipse will then present you with another dialog, asking you to import the project, using one of the options as shown in Figure 1-6.

Figure 1-6. - Import java-components as an existing Eclipse project

You now have a choice to make: you can import the java components as an existing Eclipse project, using the new project wizard, or simply as a general project. Unless you want to fully customize your project environment, I’d recommend using the first option - import an existing Eclipse project. This process will look for a .project file in the working directory (which I’ve included in each of the repositories you’ve already cloned), resulting in a new Java project named ‘piot-java-components’.

Click ‘Finish’, and you’ll see your new project added to the list of projects in the Eclipse Package Explorer, which - by default - should be on the left side of your IDE screen.

Your GDA project is now set up in Eclipse, so let’s explore the files inside. Navigate to this project in Eclipse and click on the ‘>’ to expand it further, as shown in Figure 1-7.

![](img/051213a7a9851bf581895f86e6a2becd_68_0.png)

Figure 1-7. - GDA project now setup and ready to use

# NOTE

What if you don’t like the project name? Easy. You can simply right click the ‘piot-java-components’ name. select ‘Rename…’, type the new name, and click OK. Done. Just know that I’ll continue to refer to the project by the original name throughout the book :).

You’ll notice that there’s already two files in the project: One is GatewayDeviceApp in the programmingtheiot.gda.app package, and the other is at the top level called pom.xml. The GatewayDeviceApp is a placeholder to get you started, although you may replace it with your own. I’d recommend you keep the naming convention the same, however, as the pom.xml depends on this to compile, test, and package the code. If you know your way around Maven already, feel free to make any changes you’d like.

# NOTE

For those of you new to Maven, the pom.xml is simply Maven’s primary configuration file and contains instructions for loading dependencies, their respective versions, naming conventions for your application, build instructions, and of course packaging instructions. Most of these dependencies are already included, although you may want to add your own if you find others to be useful. You’ll also notice that Maven has its own default directory structure, which I’ve kept in place for the Java repository. To learn more about these and other Maven features, I’d recommend you walk through the 5-minute Maven tutorial located at https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html.

Now, to make sure everything is in place and you can build, package, and run the GDA, do the following:

- 1. Make sure your workstation is connected to the Internet.
- 2. Build your project and create an executable package.

  - a. Right click on the project ‘piot-java-components’ in Project Workspace, scroll down to ‘Run As’, and click ‘Maven install’.
    
    - i. Since Maven will have to install any missing dependencies specified in the pom.xml, this may take a little bit to run, depending on your Internet connection speed and other factors.

  - b. Check the output in the Console at the bottom of the Eclipse IDE screen. There should be no errors, with the last few lines similar to the following:

```
[INFO] --- maven-install-plugin:2.4:install (default-install) @ gateway-device-app ---

[INFO] Installing
E:\aking\workspace\gda\programmingtheiot-java\target\gateway-device-app-0.0.1.jar
to
C:\Users\aking\.m2\repository\programmingtheiot\gda\gateway-device-app\0.0.1\gateway-device-app-0.0.1.jar

[INFO] Installing
E:\aking\workspace\gda\programmingtheiot-java\pom.xml to
C:\Users\aking\.m2\repository\programmingtheiot\gda\gateway-device-app\0.0.1\gateway-device-app-0.0.1.pom

[INFO] --------------------------------------------------
--------------------------------------------------

[INFO] BUILD SUCCESS
```

```
[INFO] ----------------------------------------
----------------------------------------
[INFO] Total time: 4.525 s
[INFO] Finished at: 2020-07-04T14:31:45-04:00
[INFO] ----------------------------------------
----------------------------------------
```

# 3. Run your GDA application within Eclipse.

- a. Right click on the project ‘programmingtheiot-java’ again, scroll down to ‘Run As’, and this time click ‘Java application’.

- b. Check the output in the Console at the bottom of the Eclipse IDE screen. As with your Maven build, there should be no errors, with the output similar to the following:

```
Jul 04, 2020 3:10:49 PM
programmingtheiot.gda.app.GatewayDeviceApp
initConfig INFO: Attempting to load
configuration.

Jul 04, 2020 3:10:49 PM
programmingtheiot.gda.app.GatewayDeviceApp
startApp INFO: Starting GDA...

Jul 04, 2020 3:10:49 PM
programmingtheiot.gda.app.GatewayDeviceApp
startApp INFO: GDA ran successfully.
```

At this point, you’re ready to start writing your own code for the GDA. Now let’s get your development workstation setup for the CDA.

## Setup Your CDA Project

This process will mimic the GDA setup process, but requires the addition of PyDev to Eclipse. Here’s a summary of activities to get you started.

If not already running, launch the Eclipse IDE. In a separate window or screen, open your web browser and navigate to https://marketplace.eclipse.org/content/pydev-python-ide-eclipse. Drag the PyDev “Install” icon from the web page and drop it near the top of the Eclipse IDE (you’ll see a green ‘plus’ icon show up, which is the indicator you can drop it into the IDE). Eclipse will then automatically install PyDev and its dependencies for you.

Once PyDev is installed, you can switch the Python interpreter to use the virtualenv environment you created in the previous section. Select ‘Preferences -> PyDev -> Interpreters -> Python Interpreter’. Eclipse will present a dialog similar to that shown in Figure 1-8.

### Figure 1-8. - Add a new Python interpreter

Then, simply add a new interpreter using the ‘Browse for python/pypy.exe’ selection and provide the relevant information in the next popup window. Once complete, select the virtualenv interpreter and click ‘Up’ until it’s at the top of the list. At this point virtualenv will be your default Python Interpreter, as Figure 1-9 indicates. Click ‘Apply and Close’.

![](img/051213a7a9851bf581895f86e6a2becd_73_0.png)

![](img/051213a7a9851bf581895f86e6a2becd_74_0.png)

Figure 1-9. - Virtualenv Python interpreter now set as default

Once these steps are complete, select ‘File -> Import’, and import the python-components Git repository you’ve already cloned from GitHub. Again, this is nearly identical to the previous steps shown in Figures 1-5, 1-6, and 1-7, except you’ll import the python-components Git repository you cloned from GitHub.

On my workstation, the repository I want to import is located at E:\aking\programmingtheiot\python-components. As with the GDA, your repository name will likely be different, so be sure to use the correct path. I’ve also included the Eclipse .project file within this repository, so you can simply import it as an Eclipse project. This one will default to Python, so will use PyDev as the project template. Again, you can import any way you’d like, but my recommendation is to use import it as you did with the GDA.

Once you complete the import process, you’ll notice a new project in your Package Explorer named ‘piot-python-components’. You now have the CDA components set up in your Eclipse IDE.

To view the files inside, navigate to ‘piot-python-components’ and click on the ‘>’ to expand it further, as shown in Figure 1-10.

![](img/051213a7a9851bf581895f86e6a2becd_75_0.png)

Figure 1-10. - CDA project now setup and ready to use
