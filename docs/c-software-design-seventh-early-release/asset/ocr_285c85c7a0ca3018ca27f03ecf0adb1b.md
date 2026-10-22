```
// ... Many database-specific functions
};

class Database
{
public:
    explicit Database( std::unique_ptr<DatabaseEngine> engine );
    // ... Many database-specific functions

private:
    std::unique_ptr<DatabaseEngine> engine_;
};

// The database is unaware of any implementation details and
// requests them
//   via its constructor from outside -> Strategy design pattern
Database::Database( std::unique_ptr<DatabaseEngine> engine )
    : engine_{ std::move(engine) }
{}
```

The actual type of `DatabaseEngine` is passed in from the outside (22), making this a good example of the *Strategy* design pattern.

Figure 7-3 shows the dependency graph for this example. Most importantly, the `Database` class is on the same architectural level as the `DatabaseEngine` abstraction, thus providing others with the opportunity to implement the behavior (e.g. in form of the `ConcreteDatabaseEngine`). Since `Database` is only depending on the abstraction, there is no dependency on any specific implementation.