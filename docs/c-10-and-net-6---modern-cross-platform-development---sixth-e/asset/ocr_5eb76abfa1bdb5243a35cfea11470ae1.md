# Building console apps using Visual Studio Code

The goal of this section is to showcase how to build a console app using Visual Studio Code.

If you never want to try Visual Studio Code or .NET Interactive Notebooks, then please feel free to skip this section and the next, and then continue with the *Reviewing the folders and files for projects* section.

Both the instructions and screenshots in this section are for Windows, but the same actions will work with Visual Studio Code on the macOS and Linux variants.

The main differences will be native command-line actions such as deleting a file: both the command and the path are likely to be different on Windows or macOS and Linux. Luckily, the dotnet command-line tool will be identical on all platforms.

# Managing multiple projects using Visual Studio Code

Visual Studio Code has a concept named a **workspace** that allows you to open and manage multiple projects simultaneously. We will use a workspace to manage the two projects that you will create in this chapter.

# Writing code using Visual Studio Code

Let's get started writing code!

+   1. Start Visual Studio Code.
+   2. Make sure that you do not have any open files, folders, or workspaces.
+   3. Navigate to **File** | **Save Workspace As**....
+   4. In the dialog box, navigate to your user folder on macOS (mine is named `markjprice`), your Documents folder on Windows, or any directory or drive in which you want to save your projects.
+   5. Click the **New Folder** button and name the folder Code. (If you completed the section for Visual Studio 2022, then this folder will already exist.)
+   6. In the Code folder, create a new folder named Chapter01-vscode.
+   7. In the Chapter01-vscode folder, save the workspace as Chapter01.code-workspace.
+   8. Navigate to **File** | **Add Folder to Workspace**... or click the **Add Folder** button.