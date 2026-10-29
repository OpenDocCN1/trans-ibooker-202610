
If you click the executable file now while the hacker program is running, you will see that nothing happens on the screen, as no pop-up console is displayed, however, the connection will be established in the background. You can see that the executable is running in the background by opening Task Manager:

![](img/d31345bbfd15f63be2920f0de5fa30dc_173_0.png)

Figure 8.7 – Background process

In the background processes in the preceding screenshot, you can see that the program is running, and a connection will be established. Note that if your program doesn't run commands properly on the victim machine, go to your victim program and where you are executing commands on the system and add the following parameter: stdin=subprocess.DEVNULL. The complete command will look like this:

```
output = subprocess.run(["powershell.exe", hacker_command], shell=True, capture_output=True, stdin=subprocess.DEVNULL)
```

The reason this error occurs is the standard input for the console is not handled properly. If you run any command on the hacker program, it should run properly. See the following example where I run the dir command:

![](img/d31345bbfd15f63be2920f0de5fa30dc_174_0.png)

Figure 8.8 – Executing the command in "noconsole" mode

Now we have created quite a stealthy malware, which can run in the background on the victim computer and give us control of the victim PC as well. But there is still a small problem with this program as it requires the user to click on the malware executable, which can be difficult or easy depending on the victim. If the user is not very tech-savvy, you can easily trick them into running it, otherwise, it will be difficult. Now we will move our discussion to trojans and how they work. We will also build a small trojan malware in the following section.

## Understanding trojans

In the previous section, we created an executable that can be run with a single click and you will then have a reverse connection with the victim, but this requires the victim to manually open and click on the executable. Here comes the concept of a trojan. A trojan is a malware program that hides in a very unsuspecting manner. Usually, these trojan malwares are merged or bundled together with legitimate software and run when the victim tries to open a legitimate application or file. You will see that a lot of the time, these viruses are merged with PDF or image files. Hiding malwares inside a trojan is a complicated task, since a lot of the time, the tricks that you learn are quickly patched in the updates for the software that you are using. For example, let's say that you discover a vulnerability in software that allows you to embed malware in a file. Unless you are the first one to discover this vulnerability, it is quite possible that this will be patched in a day or two.

## Adding an icon to an executable

If we take a look at our executable that we developed in the previous section, it has a Python icon, which can make it look like it's a Python executable. This is not very helpful for hacking purposes as it can easily be detected. One way is to add an image icon to the executable to make it look like an image instead of an executable. This will make it seem like the user is clicking to open an image while, instead, they will be running the executable. We can add the icon using pyinstaller. To do this, we need an image with a .ico extension.

Take any image and convert the extension to .ico. You can use any online tool to convert it and it should be easy. I will be using the following example website to convert my image to ICO format: online-convert.com.

Once you are done converting the image, place the converted file into the same directory as where your victim malware program is located. Once done, you can use the following command to add an icon to your executable.

You can name the file icon.ico and write the following command for pyinstaller:

```
pyinstaller --onefile --noconsole -icon=icon.ico advanced_malware.py
```

If you open the dist folder, you will see that, now, instead of a Python icon, your executable will have a different icon, depending on the image that you chose. My file looks like this:

![](img/d31345bbfd15f63be2920f0de5fa30dc_175_0.png)

In the preceding screenshot, you can see that the icon has changed now and it is now easy to trick the victim into clicking this file. Now if you click this file, you will see that it runs a background connection to the hacker, which you can verify by using Task Manager from Windows.

## Creating your own trojan

The preceding trojan will work in some cases and it should be enough. However, when the user clicks and nothing happens on the victim computer, the user might guess that something is wrong. Ideally, we would want to open an image when the user clicks on the executable and to create a simultaneous connection back to the hacker. So, the user thinks that they have just opened the image, when in fact, they have opened the image and also created the reverse shell to the hacker. In this section, we will try to further hide our malware.

To create a malware trojan, you will need four items, such as the following:

- 1. Your malware executable with the icon
- 2. The *WinRAR* program
- 3. The image to be used for the icon in .jpg format
- 4. The icon image with the .ico extension

Install the *WinRAR* software from this website: https://www.win-rar.com/.

The process should be simple. Once done, copy the executable (1), image (3), and icon (4) to a new folder. I have created a new folder called trojan and pasted all three items in it:

![](img/d31345bbfd15f63be2920f0de5fa30dc_176_0.png)

Figure 8.10 – Trojan contents

The first advanced_malware.exe is the executable, the second is the icon file, and the third is the .jpg image for the icon.

Now select all three files and right-click to select the Add to archive option:

![](img/d31345bbfd15f63be2920f0de5fa30dc_177_0.png)

Figure 8.11 – Add to archive

This will open a new dialog box. It will look like this:

![](img/d31345bbfd15f63be2920f0de5fa30dc_178_0.png)

Figure 8.12 – WinRAR dialog box

Let's rename the file to wallpaper.jpg. Select the compression method of Best and check the Create SFX archive box. Now go to the Advanced tab and open SFX options.

It will open a new dialog box. Go to the Update tab and select Extract and update files and Overwrite all files:

Then go to the Modes tab and check Unpack to temporary folder and also select Hide all for Silent mode:

![](img/d31345bbfd15f63be2920f0de5fa30dc_180_0.png)

Figure 8.15 – SFX mode

Finally, go to the Setup tab and write the following in the Run after extraction field:

![](img/d31345bbfd15f63be2920f0de5fa30dc_180_1.png)

Figure 8.16 – Run after extraction

This will allow us to run the files in sequence after we are done extracting. First, we will open the actual image that is shown to the victim, and in the background, we will also run the malware, which will give the hacker a reverse connection.

Now everything is set up as per our needs. Just click OK to create the compressed file and it will create a new file in the same folder with the name wallpaper.jpg. On the surface, it looks like a normal image file but if you open it, you will see that it creates a reverse connection, if you have the hacker server running:

![](img/d31345bbfd15f63be2920f0de5fa30dc_181_0.png)

Figure 8.17 – Trojan

In Figure 8.16, you can see that we have created a trojan named wallpaper.jpg. If you look very closely, you can see that it has the type Application but in Windows, extensions are hidden by default and we have added the name wallpaper.jpg to it so it looks like an image. And if you click on the image, it will open the image and concurrently create a reverse connection with the hacker. Go ahead and try it yourself. Our current malware attack has only worked over a private IP until now. In the next section, we will learn how to perform the same attack over a public IP.

## Attack over a public IP

So far, all the attacks we have done are on the local network. This requires you, the hacker, and the victim to be connected to the same network. This will likely not be the case for a lot of attack scenarios. This is where a public IP comes into play. We have already learned about public and private IP addresses while discussing an introduction to networking. In order to create a successful attack, we need to know the public IP of the hacker. On the hacker machine, you can find your public IP by going to google.com and typing my public ip and this will display your public IP address provided you are not using any VPN or network masking schemes. It will be a similar 32-bit address, which is provided to you by your ISP. My public IP is 31.38.10.X. The last 8 bits are masked due to privacy issues. Your IP will be different depending on where you live. It should not be very difficult to find. Once you have your public IP address, go to the victim program and instead of writing the private IP of the hacker, write the public IP of the hacker.
