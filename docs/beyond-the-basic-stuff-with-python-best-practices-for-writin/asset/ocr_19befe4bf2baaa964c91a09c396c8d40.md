```
```python
class AsciiArt:
    def __init__(self, characters):
        self._characters = characters

    @classmethod
    def fromFile(cls, filename):
        with open(filename) as fileObj:
            characters = fileObj.read()
            return cls(characters)

    def display(self):
        print(self._characters)

    # Other AsciiArt methods would go here...

face1 = AsciiArt(' _____ \n' +
                 '  . .  \n' +
                 '  \___/  \n' +
                 ' |_____|')
face1.display()

face2 = AsciiArt.fromFile('face.txt')
face2.display()
```
```

The AsciiArt class has an __init__() method that can be passed the text characters of the image as a string. It also has a fromFile() class method that can be passed the filename string of a text file containing the ASCII art. Both methods create AsciiArt objects.

When you run this program and there is a face.txt file that contains the ASCII art face, the output will look something like this:

![](bbox=[0.25, 0.66, 0.35, 0.75])

The fromFile() class method makes your code a bit easier to read, compared to having __init__() do everything.

Another benefit of class methods is that a subclass of AsciiArt can inherit its fromFile() method (and override it if necessary). This is why we call cls(characters) in the AsciiArt class's fromFile() method instead of AsciiArt(characters). The cls() call will also work in subclasses of AsciiArt without modification because the AsciiArt class isn't hardcoded into the method. But an AsciiArt() call would always call AsciiArt class's __init__() instead of the subclass's __init__(). You can think of cls as meaning "an object representing this class."