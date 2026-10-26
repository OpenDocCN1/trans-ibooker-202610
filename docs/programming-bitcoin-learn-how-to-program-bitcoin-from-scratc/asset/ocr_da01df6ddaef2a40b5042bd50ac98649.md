# Combining the Script Fields

The Script object represents the command set that requires evaluation. To evaluate a script, we need to combine the ScriptPubKey and ScriptSig fields. The lockbox (ScriptPubKey) and the unlocking mechanism (ScriptSig) are in different transactions. Specifically, the lockbox is where the bitcoins are received, and the unlocking script is where the bitcoins are spent. The input in the spending transaction points to the receiving transaction. Essentially, we have a situation like Figure 6-6.

![](bbox=[0.13, 0.24, 0.84, 0.39])

Figure 6-6. Combining the ScriptPubKey and ScriptSig

Since the ScriptSig unlocks a ScriptPubKey, we need a mechanism by which the two scripts combine. To evaluate the two together, we take the commands from the ScriptSig and ScriptPubKey and combine them as in Figure 6-6. The commands from the ScriptSig go on top of all the commands from the ScriptPubKey. Instructions are processed one at a time until no commands are left to be processed (or the script fails).

# Coding the Combined Instruction Set

The evaluation of a script requires that we take the ScriptSig and ScriptPubKey, combine them into a single command set, and execute the commands. To do this, we require a way to combine the scripts:

```
class Script:
    ...
    def __add__(self, other):
        return Script(self.cmds + other.cmds)
```

① We are combining the command set to create a new, combined Script object.
We will use this ability to combine scripts for evaluation later in this chapter.

# Standard Scripts

There are many types of standard scripts in Bitcoin, including the following: