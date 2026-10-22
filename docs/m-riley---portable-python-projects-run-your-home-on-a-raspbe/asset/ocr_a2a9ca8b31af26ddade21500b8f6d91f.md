# Creating a Sleep-Timer Script

If you installed Visual Studio Code, you’re welcome to use that editor to open the empty twitchtimer.py file we created earlier. But given the brevity of this project’s Python script, it may be easier to use the Nano text editor for now:

```
$ nano twitchtimer.py
```

Either manually enter or copy and paste the following Python code into the editor window:

```
twitchtimer/twitchtest.py

① import os, subprocess, signal, sys, time
② duration = int(sys.argv[1])*60
③ cmd = "streamlink twitch.tv/mst3k 720p"
④ twitchstream = subprocess.Popen(cmd,shell=True)
⑤ time.sleep(duration)
⑥ os.killpg(os.getpgid(twitchstream.pid), signal.SIGTERM)
```

① The first line of code imports the Python standard libraries we’ll depend upon in later parts of the script.

② We’ll pass a numeric value as an argument when we run our script. This integer will indicate the number of minutes we want to stream before forcing the stream to quit. Python’s time.sleep() method will pause script execution for the number of seconds asssigned to it. Since the argument being passed in is minutes, we need to convert that value to seconds by multiplying it by 60.

③ We’ll create a string variable called cmd that will store the command we