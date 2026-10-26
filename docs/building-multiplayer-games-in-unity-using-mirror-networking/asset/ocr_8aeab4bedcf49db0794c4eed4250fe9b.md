+   - There should be a text input and Send Message button.
+   - The application should use Mirror Networking’s Network Messages.
+   - The application should display a notification in the chat that shows whenever a user joins the conversation with a timestamp.
+   - All user messages should display timestamps of messages.
+   - All user messages should display the sender’s display name.
+   - Messages sent by a user should appear differently for that user. IE. A different chat box color.

# Bonus Challenge

Up for a challenge? Here are some additional tasks to complete for this challenge to push your understanding of Mirror Networking:

+   - Users should directly message other users in a separate chat window by clicking on the target user within the left user menu. This chat should also persist and be accessible between sessions.
+   - Allow for users to create a group chat. The group chat name should show up within the left user menu to all members within the group chat.
+   - Allow for users to choose an image file and upload it to the chat. You can handle this however you’d like, as long as the image persists between sessions.
+   - Allow a user to block another user. That user’s messages should show up as “Blocked Message,” and the blocked user should not have the ability to send direct messages to the blocking user.
+   - Ensure that your user interface can adjust to multiple screen resolutions.
+   - Deploy a build to Android, and have the android client able to connect to the chat. This Android client should function identically to the desktop client.
+   - Sanitize your user inputs to prevent breaking your database and application by removing TMPText markdown and glitchy ASCII symbols (see https://en.wikipedia.org/wiki/Zalgo_text ).