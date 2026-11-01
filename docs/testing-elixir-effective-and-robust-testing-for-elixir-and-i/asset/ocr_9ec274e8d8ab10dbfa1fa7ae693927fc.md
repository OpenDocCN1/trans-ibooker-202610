```
- phone_number: Faker.Phone.EnUs.phone(),
- }
- end
end
15:
```

Unless you need more than one factory file (which can be useful for separation of concerns as your application grows), sticking with the name Factory should be just fine. At line 2, our module is “using” ExMachina.Ecto and passing it the name of our application’s repo, TestingEcto.Repo. This means that this factory will be usable specifically with that repo. In applications that connect to more than one database, you’ll want to have multiple factories, naming each of them something more specific.

We’ve added a single factory function (line 5), following an ExMachina convention: the name of the schema followed by “_factory”. ExMachina uses metaprogramming under the hood; and to make it work, this convention is a must. It ends up creating a slight disconnect when you actually use the factory function, which we’ll call out later. For now, just know that any functions in the factory file that need to work with ExMachina will need to have “_factory” appended.

The function itself is pretty simple; it just returns a schema struct with pre-populated values. If we were to call this function by name, it would return a struct. That in itself is useful, but it isn’t anything we need another library for. As long as you follow the naming convention, ExMachina offers a lot of helper functions, ones that we’ll be using throughout the rest of this chapter. The different functionality it provides works in two phases. First it generates a data set from the factory defaults and any overrides passed in. When the data is ready, it performs the action from the function call, such as inserting a record or creating a string-keyed map. While the initial data set is based off the same struct, the return value and side effects are determined by the