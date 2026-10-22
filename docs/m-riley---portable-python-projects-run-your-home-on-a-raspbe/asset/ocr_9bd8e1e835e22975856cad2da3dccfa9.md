address to lower case, and convert the dashes in between each pair of characters to colons. Remember to replace the placeholder xx pairs in the example below with the actual MAC address values assigned to your Ethernet adapter:

```
$ wakeonlan xx:xx:xx:xx:xx:xx
```

For example, if your Ethernet adapter MAC value is d6:21:b4:3b:a1:c2, then the wakeonlan command would be:

```
$ wakeonlan d6:21:b4:3b:a1:c2
```

Once you’ve entered the correctly formatted MAC address from your computer in the preferred format and run the wakeonlan program, you should see your Mac or PC turn on. If it failed to do so after twenty seconds or so, check to make sure you’re using the correct Ethernet adapter MAC address. If your target desktop computer is Windows, also verify that all that adapter’s power settings, including those in the BIOS, have been set correctly. Do not proceed until you can successfully wake up your computer running the wakeonlan command.

With the local area network and computing resources correctly configured, we can continue with the project by establishing free user accounts on the IFTTT and Pushbullet services.

# IFTTT

IFTTT, which stands for If This Then That, enables the creation of custom voice-prompted workflows ridiculously easy. This is in comparison to the code and resources required if you opt to set up a stand-alone service of your own. Entire books have been written about doing just that, including Build Talking Apps for Alexa [Wal22]. I strongly encourage enthusiastic developers interested in making their own services this way to pursue this path. But for those who want to quickly create a very basic working service, IFTTT offers