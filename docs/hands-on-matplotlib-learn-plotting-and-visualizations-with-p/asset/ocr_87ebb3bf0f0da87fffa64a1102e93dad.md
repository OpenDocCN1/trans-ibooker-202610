Note that every instance of the Jupyter Notebook server will have its own token, so the token shown in the book will not work with your notebook. The token is valid only for that server process.

So, if you follow any one of the routes explained earlier, you will see a home page tab in the browser window, as shown in Figure 1-15.

![](bbox=[0.108, 0.235, 0.926, 0.5])

Figure 1-15. A new home page tab of Jupyter Notebook

As you can see, there are three tabs on the web page: Files, Running, and Clusters. The Files tab shows the directories and files in the directory from where you launched the notebook server from the command prompt. In the previous example, I executed the command jupyter notebook from lxterminal of my Raspberry Pi. And the current working directory is the home directory of the pi user /home/pi. That is why you can see all the files and directories in the home directory of my Raspberry Pi computer in Figure 1-15.

In the top-right corner, you can see the Quit and Logout buttons. If you click the Logout button, then it logs out from the current session, and to log in, you again need the token or URL with the embedded token from the notebook server log, as discussed earlier. If you click the Quit button, then it stops the notebook server process running at the command prompt and displays the modal message box shown in Figure 1-16.