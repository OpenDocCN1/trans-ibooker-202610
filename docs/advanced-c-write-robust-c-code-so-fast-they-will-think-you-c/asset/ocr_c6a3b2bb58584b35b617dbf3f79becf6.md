```
std::mutex g_AddMux;
std::mutex g_RemoveMux;
bool g_AddNotified = false;;
bool g_RemoveNotified = false;
Persons g_PeopleInside;
Persons g_PeopleInqueue;
Persons g_CreatedPersons;
PersonGenerator generator(g_CreatedPersons, g_CondVarAddPerson, g_CondVarRemovePerson, g_AddMux, g_RemoveMux, g_AddNotified, g_RemoveNotified);
Watchman watchman(g_CondVarAddPerson, g_CondVarRemovePerson, g_AddMux, g_RemoveMux, g_AddNotified, g_RemoveNotified, g_PeopleInside, g_PeopleInqueue, g_CreatedPersons);
}

char a;
std::cin >> a;
return 0;
}
```