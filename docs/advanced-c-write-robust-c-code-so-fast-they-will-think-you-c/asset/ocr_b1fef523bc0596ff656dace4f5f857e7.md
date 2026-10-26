8. Create the characters and manage them in a container such as a **vector**. Create the following two methods in the file, before the `main()` function:

```
void createCharacters(std::vector<ICharacter*>& cast)
{
    cast.push_back(new Wizard("Gandalf"));
    cast.push_back(new Healer("Glenda"));
    cast.push_back(new Warrior("Ben Grimm"));
}

void freeCharacters(std::vector<ICharacter*>& cast)
{
    for(auto* character : cast)
    {
        delete character;
    }
    cast.clear();
}
```

9. Replace the content of `main()` with the following code:

```
int main(int argc, char**argv)
{
    std::cout << "\n------ Exercise 1 ------\n";

    std::vector<ICharacter*> cast;

    createCharacters(cast);
    for(auto* character : cast)
    {
        character->speak();
    }
    for(auto* character : cast)
    {
        character->act();
    }
    freeCharacters(cast);

    std::cout << "Complete.\n";
    return 0;
}
```