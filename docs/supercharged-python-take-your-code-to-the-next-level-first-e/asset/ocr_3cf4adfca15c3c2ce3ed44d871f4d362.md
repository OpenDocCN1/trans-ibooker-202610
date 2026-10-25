```
class Dog(Mammal):
    def speak(self):
        print('ARF!')
    def __init__(self, name, size, breed):
        super().__init__(name, size)
        self.breed = breed

class class_name(base_class1, base_class2, ...):
    statements

class Dog(Mammal, Pet, Carnivore):
    def speak(self):
        print('ARF!')
    def __init__(self, name, size, breed):
        Mammal.__init__(self, name, size)
        self.breed = breed

def __init__(self, name, size, nickname, breed):
    Mammal.__init__(self, name, size)
    Pet.__init__(self, nickname)
    self.breed = breed
```