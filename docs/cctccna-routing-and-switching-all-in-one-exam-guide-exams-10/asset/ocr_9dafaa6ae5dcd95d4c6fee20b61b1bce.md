![](bbox=[0.14, 0.11, 0.24, 0.17])

**EXAM TIP** You should remember that the System Configuration Dialog script is started when the router boots up and there is no configuration in NVRAM, or you use the **setup** command from privileged EXEC mode. Also, know the three options at the end of the Setup dialog script. And remember that you can press ctrl-c to abort the script.

# Exiting Setup Mode

After you answer all of the script’s configuration questions, you are shown the router configuration the script created using your answers to the script’s questions. Note that the IOS hasn’t yet activated the configuration file. Examine the configuration closely and then make one of the three choices shown in Table 6-4. Also, if you enter **1** as your option, when the script starts over again, the information that you previously entered appears in brackets and will be the default values when you press the enter key on an empty line.

| Option | Description |
|---|---|
| 0 | Discard the script’s configuration and return to privileged EXEC mode. |
| 1 | Return to the beginning of the script. |
| 2 | Activate the script’s configuration, save the configuration to NVRAM, and return to privileged EXEC mode. |

Table 6-4 Options at the End of the System Configuration Dialog

![](bbox=[0.14, 0.78, 0.22, 0.84])

**VIDEO 6.03.** The digital resources that accompany this book contain a multimedia demonstration of using the System Configuration Dialog on