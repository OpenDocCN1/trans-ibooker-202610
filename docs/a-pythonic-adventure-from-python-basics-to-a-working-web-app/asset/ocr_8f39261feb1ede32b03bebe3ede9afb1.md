![](bbox=[0.12, 0.05, 0.88, 0.31])

"When you work with a file in your computer you use its name. You tell your editor program to open a file called 'drinks.txt'. Your editor program then reads the file and shows you its content. Then you edit the file and save it which means you write the file on the computer disk. So far, so good?" Simon asked.

"Yes," Erik said. "But disks are used only in very old computers. In my computer it’s called SSD drive and it’s not a disk anymore. My friend Alex told me."

"You’re absolutely right!" Simon was glad to hear that from his brother. "Yes, it’s SSD now in most of the computers and yes, it’s not a disk and it is not spinning. By the way, maybe your friend Alex would like to join us? It seems that he is interested in computers. Maybe he wants to learn some programming too?"

"I’ll ask him," Erik said. "But let’s get back to my files. I see that you wrote 'computer' and 'Python' at the top of your picture. What does that mean?"

"That means," Simon started his explanation, "that in Python if you want to work with a file you have to create a special object which is usually called a file handle. You use this object to read and write the file. You use a function called open() to create such an object. You call the function open() and you pass the file name as an argument. In our case it will be open("drinks.txt"). The function returns the file handle which you put in a variable. In this case the variable is called f, but you can use any name here."

"Why is it so complicated?" Erik asked. "Why can’t we just use the file name?"

"Yes, it looks a bit complicated for the first time. The reason for that is that the file name is just a string, remember? When we want to read from the file we want to read from the file with that name, not from the string. The file name and the file itself are different things. When we use the