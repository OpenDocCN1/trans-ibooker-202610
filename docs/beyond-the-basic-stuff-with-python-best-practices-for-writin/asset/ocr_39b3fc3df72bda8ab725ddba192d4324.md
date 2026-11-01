program, and then install it. On Linux, you can install either Meld by using the sudo apt-get install meld command or Kompare by using the sudo apt-get install kompare command. On macOS, you can install tkdiff by using commands that first install and configure Homebrew (a package manager that installs software) and then using Homebrew to install tkdiff:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
brew install tkdiff
```

You can configure Git to use these tools by running git config diff.tool <tool_name>, where <tool_name> is winmerge, tkdiff, meld, or kompare. Then run git difftool <filename> to view the changes made to a file in the tool's GUI, as shown in Figure 12-5.

![](bbox=[0.24, 0.3, 0.9, 0.64])

Figure 12-5: A GUI diff tool, in this case WinMerge, is more readable than the text output of git diff.

Additionally, run git config --global difftool.prompt false so Git doesn't ask for confirmation each time you want to open the diff tool. If you installed a GUI Git client, you can also configure it to use these tools (or it might come with a visual diff tool of its own).

# How Often Should I Commit Changes?

Even though version control allows you to roll back your files to an earlier commit, you might wonder how often you should make commits. If you commit too frequently, you'll have trouble sorting through a large number of insignificant commits to find the version of the code you're looking for.