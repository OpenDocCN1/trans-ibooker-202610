+   - assert {:error, :not_found} = Users.get(Ecto.UUID.generate())
+   - end
+   - end

These tests are pretty easy to understand. Because the function under test is reading from the database, our test code will have to insert data before the exercise step. We’re using our factory to insert the data into the database at line 3. One of the many benefits of using a factory is that it provides an easy and consistent way to insert data into the database without relying on your own application code.

It’s very important to note, though, that because it isn’t using your application code, the inserted data won’t be run through your changeset functions. If rules about the data are present in the changeset logic but aren’t reflected in the database, you need to make sure that your factory function only returns valid data. That’s testing a read function.