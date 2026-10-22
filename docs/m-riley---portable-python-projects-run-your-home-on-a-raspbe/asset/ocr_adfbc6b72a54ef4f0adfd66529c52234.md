+   ① Import the gTTS library used to perform the TTS conversion.
+   ② Pass the test_text text string along with language and playback speed parameters into the gTTS object, perform the conversion, and save the results to the output object.
+   ③ Save the audio content stored in the output object to the output.mp3 audio file.

Save and execute the ttstest.py script. Using the File Browser, open the output.mp3 file located in the same directory from where you ran the script and listen to the playback.

One annoying limitation with the current gtts implementation is its inability to selectively alter the TTS playback speed. I find even after setting gtts’s slow parameter equal to False that the playback result is still too slow for my fast-paced listening preference. Fortunately there’s an easy fix to this problem.