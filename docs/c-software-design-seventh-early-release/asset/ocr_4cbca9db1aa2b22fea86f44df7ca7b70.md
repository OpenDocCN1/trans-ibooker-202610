```
// ...
}
};
```

Widget and Gadget both require access to the system-wide Database. For that reason they call the Database::instance() function and subsequently the read() and write() functions.

Since they use the Database and thus depend on it, we would like them to reside in architecture levels below the level of the Database Singleton. That is because, as you remember from “Guideline 2: Design for Change”, we can only call it a proper architecture if all dependency arrows run towards the high levels (see Figure 10-2).