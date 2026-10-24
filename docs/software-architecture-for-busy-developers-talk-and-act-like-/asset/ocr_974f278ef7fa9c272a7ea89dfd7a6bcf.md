On the left-hand side of Figure 5.41, you can see a generic repository represented by IAsyncRepository and a business-specific one represented by IDemoRepository. The DemoController class gets an instance of IDemoRepository through DI, for which the concrete class is DemoRepository, which in turn inherits from the BaseRepository class.

Both BaseRepository and DemoRepository leverage DemoContext (ORM) to interact with the underlying data store. Finally, DemoEntity is simply a representation of a data entity. Since we have already touched on DI and controllers before, I will only list the most important code blocks in our next section. You can find all the code on GitHub, as explained in the Technical requirements section of this chapter.

# Repository pattern in action

Let's first start with our generic repository, as follows:

```
public interface IAsyncRepository<T> where T : class{
    Task<T> GetByIdAsync(Guid id);
    Task<IReadOnlyList<T>> ListAllAsync();
    Task<T> AddAsync(T entity);
    Task UpdateAsync(T entity);
    Task DeleteAsync(T entity);
}
```

Figure 5.42 – Generic repository

The repository is generic because it takes an input of T, which is the .NET way to handle generics. All the methods declared in the interface are also generic because they do not serve any specific business purpose. They simply represent the typical create, read, update, and delete (CRUD) operations.

This generic repository is particularly the reason why the repository pattern is subject to controversy. If you only stick to this implementation, you simply add redundant code because, by default, ORMs also come with such default CRUD operations over collections of objects.