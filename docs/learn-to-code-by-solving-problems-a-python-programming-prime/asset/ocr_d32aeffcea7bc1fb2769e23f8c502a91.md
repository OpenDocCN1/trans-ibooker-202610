One warning about f-strings: they were added in Python 3.6, which at the time of writing is still a reasonably recent version of Python. In older versions of Python, f-strings cause syntax errors.

If you use f-strings, be sure to check that the judge you’re submitting to is using Python 3.6 or newer to test your code.

Before continuing, you might like to try solving exercise 1 from “Chapter Exercises” on page 99.

# Problem #9: Song Playlist

Sometimes we don’t know in advance how much input will be provided. We’ll see in this problem that a while loop is what we need in such cases.

This is DMOJ problem ccc08j2.

# The Challenge

We have five favorite songs named A, B, C, D, and E. We’ve created a playlist of these songs and are using an app to manage the playlist. The songs start off in the order A, B, C, D, E. The app has four buttons:

+   - Button 1: Moves the first song of the playlist to the end of the playlist. For example, if the playlist is currently A, B, C, D, E, then it changes to B, C, D, E, A.
+   - Button 2: Moves the last song of the playlist to the beginning of the playlist. For example, if the playlist is currently A, B, C, D, E, then it changes to E, A, B, C, D.
+   - Button 3: Swaps the first two songs of the playlist. For example, if the playlist is currently A, B, C, D, E, then it changes to be B, A, C, D, E.
+   - Button 4: Plays the playlist!

We’re provided a user’s button presses. When the user presses button 4, output the order of songs in the playlist.

# Input

The input consists of pairs of lines, where the first line of a pair gives the number of a button (1, 2, 3, or 4), and the second gives the number of times that the user pressed this button (between 1 and 10). That is, the first line is the number of a button, the second line is the number of times