contains a set of folders and files for the so-called Gradle *wrapper*. This wrapper is a script file (called `gradlew` for Unix-like systems and `gradlew.bat` for Windows) that provides the following benefits:

+   - Build a project without the need to install Gradle on the local machine.
+   - Force to use a given version (which can be different from the locally installed instance of Gradle).
+   - Upgrade to a new version easily by changing the wrapper artifacts (in folder `gradle/wrapper`).

> **NOTE**
> It is out of the scope of this book to explain all the features provided by Maven and Gradle. Nevertheless, you can find more information about their build lifecycle and typical commands. For further information, consider reading the official Maven and Gradle documentation.

# Dependencies

The dependencies of a software project are the required libraries or plugins. Among other features, build tools enable the automated management of the project dependencies. To that aim, we need to specify the *coordinates* of such dependencies in the project configuration file (see the following subsections for specifics on Maven and Gradle). The coordinates of a Java project are a group of three labels that univocally identify this project (e.g., a library, plugin, etc.), namely:

+   - `groupId`: Organization, company, person, etc., that created the project.
+   - `artifactId`: Unique name that identifies the project.
+   - `version`: Particular release of the project. By default, I recommend you use the latest version of every release. For that reason, I use Dependabot to maintain these versions automatically.