On the hacker side, we will define the exact same identifier, so we can match it. Now, instead of just receiving 1,024 bytes, we will add a while loop, which will continuously receive data and store it in an array until we find the identifier, and then we will remove the identifier and store the rest of the result.

Take a look at the following receiving code:

```
full_command_result = b""
while True:
    chunk = hacker_socket.recv(1048)
    if chunk.endswith(IDENTIFIER.encode()):
        chunk = chunk[:-len(IDENTIFIER)]
        full_command_result += chunk
        break
    full_command_result +=chunk
print(full_command_result.decode())
```

We define a full_command_result variable that will hold the complete result. Then we write a loop to read the buffer continuously until we reach the identifier. Once the identifier is reached, we remove the identifier from the result, add the remaining bytes to full_command_result, break the loop, and finally decode it to print. The complete program for the hacker is shown here: https://github.com/PacktPublishing/Python-Ethical-Hacking/blob/main/example08-hacker-malware/hacker.py.

Similarly, the complete program for the victim is shown here: https://github.com/PacktPublishing/Python-Ethical-Hacking/blob/main/example09-victim-malware/victim.py.