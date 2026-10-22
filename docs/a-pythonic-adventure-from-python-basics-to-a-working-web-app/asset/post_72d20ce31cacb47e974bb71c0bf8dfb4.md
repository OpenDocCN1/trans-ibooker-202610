
program, you don’t know Python, but you want to add those flavors and toppings to the menu. You, as a programmer, should give the manager an easy way to add something to the menu."

"How do you suggest to do that?" Erik asked. He knew already that Simon has something in mind.

"I think we should put menus in files and read your lists from those files."

"Like in Word documents?" Erik asked.

"Yes, almost," Simon said. "Your program will open those files and read from them. I think the easiest will be to have one file per menu. One file will have all drinks, another one—all flavors, and another—all toppings. Then your manager will just edit those files instead of editing your Python code. Sounds good?"

"Yes, interesting," Erik said. He wondered how his Python program would open files the same way Word does.

"Great," Simon said. "This is what we are going to do tomorrow. Take some rest now."

## New things you have learned today

- **Users make mistakes**
  You have learned that user not always follow the directions you give them in your program. You have to be ready for that and check their input for errors, wrong types, etc.
- **Indentation**
  When you create a *block* in Python (like `while`, for example) you have to make sure all code in the block is *indented*, i.e. shifted to the right.
- **How to exit from a menu**
  You have to give your users a way to exit from each menu. For example, if they don’t want to order any topping and they want to skip it.

## Code for this chapter

You can find the code for this chapter here:
https://github.com/pavelanni/pythonicadventure-code/tree/main/ch04

## Working with files: Being a shop manager

### In this chapter

- Erik learns what a coffee shop manager needs
- Erik reads his menus from files
- Erik writes his second Python function

"Last time you said something about a coffee shop manager," Erik started with his brother the next day. "Something about changing menus. I forgot."

"Yes, I said it might be good if your coffee shop manager could change the menus without going into Python code," Simon said.

"Yes, good idea," Erik said. "Not everyone knows Python."

"My idea is to create simple text files for each menu—call them `drinks.txt`, `flavors.txt` and `toppings.txt`. Then your program can read from those files and create lists from the items in the files."

"Why did you name them all with the `.txt` at the end?" Erik asked. "Shouldn’t they be `.docx` so the manager could edit them in Microsoft Word?"

"Good point," Simon said. "Yes, the manager might be more familiar with Word, but in our case we need just a plain text file, without fonts, or headers, or table of contents. It’s similar to your Python code—these files should have nothing but lines of plain text, and the manager should use a plain text editor to work with them. When I name them with `.txt` at the end I tell the operating system—whether it’s Windows, or macOS, or Linux-- that this file should be opened with a plain text editor and not a word processor like Word. In all those systems there is always a text editor that can edit these files. And you can also install another application for that, like we did with Mu Editor. Also, for Python it’s much easier to read from a plain text file than from a .docx file."

"Let’s create these files," Simon continued. "You can use your Mu Editor for that. Just don’t forget to add the .txt extension when saving the files. Otherwise it will automatically add .py. Create a new file, enter your drinks, each on a separate line, and save it with the name drinks.txt. Then do the same for flavors and toppings."

Erik started working. After several minutes he’s got three files.

### Listing 5.1 drinks.txt

```
coffee
chocolate
decaf
```

### Listing 5.2 flavors.txt

```
caramel
vanilla
peppermint
raspberry
plain
```

### Listing 5.3 toppings.txt

```
chocolate
cinnamon
caramel
```

YOUR TURN   Create your own menu files
Create the text files with menu items like Erik just did. Make sure they are plain text files. Try to use your favorite flavors and toppings in your menus.

"Now what?" he asked his brother.

"Now let’s learn how to work with files in Python. Let’s create a new program for that. We’ll practice a little bit and then we’ll add this code to our main program. I usually do this when I learn something new—try it in a separate simple program before adding to the main application."

Simon continued, "Create another file in the editor and save it as files.py."

Erik has done that several times already so it took him just a couple seconds.

"Working with files is a difficult topic, so let me start with a diagram," Simon said.

![](img/f6295b544c84568800930d6ec95963ae_70_0.png)

"When you work with a file in your computer you use its name. You tell your editor program to open a file called 'drinks.txt'. Your editor program then reads the file and shows you its content. Then you edit the file and save it which means you write the file on the computer disk. So far, so good?" Simon asked.

"Yes," Erik said. "But disks are used only in very old computers. In my computer it’s called SSD drive and it’s not a disk anymore. My friend Alex told me."

"You’re absolutely right!" Simon was glad to hear that from his brother. "Yes, it’s SSD now in most of the computers and yes, it’s not a disk and it is not spinning. By the way, maybe your friend Alex would like to join us? It seems that he is interested in computers. Maybe he wants to learn some programming too?"

"I’ll ask him," Erik said. "But let’s get back to my files. I see that you wrote 'computer' and 'Python' at the top of your picture. What does that mean?"

"That means," Simon started his explanation, "that in Python if you want to work with a file you have to create a special object which is usually called a file handle. You use this object to read and write the file. You use a function called open() to create such an object. You call the function open() and you pass the file name as an argument. In our case it will be open("drinks.txt"). The function returns the file handle which you put in a variable. In this case the variable is called f, but you can use any name here."

"Why is it so complicated?" Erik asked. "Why can’t we just use the file name?"

"Yes, it looks a bit complicated for the first time. The reason for that is that the file name is just a string, remember? When we want to read from the file we want to read from the file with that name, not from the string. The file name and the file itself are different things. When we use the function open() we create a connection between the file name and the file itself. We tell Python: 'please find the file named drinks.txt inside the computer and use it as a file.' Don’t worry too much about it right now. Sometimes the best way to understand something is to start using it."

"Okay," Erik said. He was still a bit confused about all that but we wanted to see how he can read his menus from the text files he just created.

"Now let’s write a simple Python program to work with files," Simon said. "Now go to the tab in your editor where you have your files.py program opened.

Look at my diagram: you have to call the function open(), pass the file name such as 'drinks.txt', and store the result in the variable f. Can you write it?"

![](img/f6295b544c84568800930d6ec95963ae_71_0.png)

"Let me try," Erik said and wrote this.

```
f = open("drinks.txt")
```

"Good!" Simon said. "Now you have a file object named f and you can read from it. To do that you call a method called read(). Methods look and behave very similar to functions, but methods are applied to objects. We’ll talk about objects later but for now all you have to know is that to call a method of an object you use the object’s name, then you put a dot and then the method name with parentheses, similar to calling a function. Like this: f.read(). Methods also can return something, similar to functions. So you have to store the result somewhere. What do you think this method read() will return when you call it?"

"What’s inside the file, I think," Erik answered.

"Absolutely correct!" Simon said. "Save it in a variable called drinks and then try to print it."

Erik wrote the following.
