previous chapters, but we have a few more ideas to enhance flexibility and quality of life to talk about before we’re finished.

# Prompts

There aren’t really any idioms for the bash prompt. The prompt you get by default depends on where your bash came from and how the configuration files have been tweaked, and every user likes different prompt settings. But there are some things to be aware of.

We’re not going to go into a lot of detail about how you control the prompt—that’s covered in the bash documentation, the *bash Cookbook* (recipe 16.2 and elsewhere), and all over the web. See the following:

+   - https://www.gnu.org/software/bash/manual/html_node/Controlling-the-Prompt.html
+   - http://www.bashcookbook.com/bashinfo/source/bash-4.2/examples/scripts.noah/prompt.bash
+   - https://www.tldp.org/HOWTO/Bash-Prompt-HOWTO/index.html
+   - https://sourceforge.net/projects/bashish

We will briefly cover the different prompts, because there are some new and little-known features, as shown in Table 10-1.

Table 10-1. bash prompts

| Prompt | Use | Default |
|---|---|---|
| PS0 | bash v4.4+ pre-execution prompt | None |
| PS1 | Bourne/bash primary prompt | '\s-\v\$ ' |
| PS2 | Bourne/bash secondary prompt | '> ' |
| PS3 | bash select prompt | '#? ' |
| PS4 | bash debug parameter | '+ ' |
| PROMPT_COMMAND | command before $PS1 | None |

# PS0

This prompt is displayed by interactive shells after you hit enter on a command but before the command runs. It can be useful to display a “start time” or other marker just before a command runs (see Example 10-1).