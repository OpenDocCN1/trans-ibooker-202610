```
if ((elementFromTheList = (charlist_t*) malloc(sizeof(charlist_t))) != 0)
{
    elementFromTheList->character = 0;
    elementFromTheList->nextCharacter = NULL;
}
else
{
    perror("The allocation using malloc() has failed.");
}

    return elementFromTheList;
}

// allocation free memory by the characters list
// listOfCharacters - represents a pointer for the first element within the list
void freeAllocation_CharactersList(charlist_t* listOfCharacters)
{
    charlist_t* currentCharacter = listOfCharacters;
    charlist_t* nextCharacter;

    while (currentCharacter != NULL)
    {
        nextCharacter = currentCharacter->nextCharacter;
        free(currentCharacter);
        currentCharacter = nextCharacter;
    }
}

// the function display the current list of characters
// the function will iterate through the whole list and it will print all the characters
void showCharactersList(charlist_t* list)
{
    charlist_t* nextCharacter = list;
```