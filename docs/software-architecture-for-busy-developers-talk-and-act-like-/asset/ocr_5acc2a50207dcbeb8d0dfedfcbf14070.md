```
public class DemoRepository : BaseRepository<DemoEntity>, IDemoRepository
{
    public DemoRepository(DemoContext dbContext) : base(dbContext) { }
    public async Task<IEnumerable<DemoEntity>> ListOnlyOddEntities(){
        return await Task<IEnumerable<DemoEntity>>.Run(() => {
            return _dbContext.DemoEntities.ToList().Where((c, i) => i % 2 != 0)
                as IEnumerable<DemoEntity>;
        });
    }
}
```

Figure 5.45 – Tailor-made repository

DemoRepository derives from BaseRepository and passes in the DemoEntity type. It also implements IDemoRepository through the concrete implementation of the ListOnlyOddEntities method. If you know a little bit about .NET, you have probably identified a code smell here. The odd/even check is done in memory and causes the ORM to produce a SELECT * FROM ... statement, which is never good performance-wise. I did this on purpose to show you how easy it is to write very extendable code but leave such poor constructs in it. By the end of this chapter, I will let you know what I consider to be the most important things to catch in a code-review round.

# Repository pattern wrap-up

Here are a few things to consider when using the repository pattern:

+   - Do not stick to the generic repository only. You should aim to encapsulate the business logic into more specific repositories.
+   - Do not abstract away the ORM, which is itself an abstraction of the underlying data store. Probably 99.99% of the time, you will only stick to a single ORM for the lifetime of the application.
+   - Repositories help improve testability.
+   - Keep the number of abstraction layers manageable. I'll leave it to you to evaluate what manageable means.

Let's now go through a small use case to see if you can apply what you have just learned about design patterns.