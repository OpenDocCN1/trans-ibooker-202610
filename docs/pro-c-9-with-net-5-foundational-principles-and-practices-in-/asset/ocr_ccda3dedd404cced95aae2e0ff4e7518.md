```
public List<CarViewModel> GetAllInventory()
{
    OpenConnection();
    // This will hold the records.
    List<CarViewModel> inventory = new List<CarViewModel>();

    // Prep command object.
    string sql =
        @"SELECT i.Id, i.Color, i.PetName,m.Name as Make
            FROM Inventory i
            INNER JOIN Makes m on m.Id = i.MakeId";
    using SqlCommand command =
        new SqlCommand(sql, _sqlConnection)
    {
        CommandType = CommandType.Text
    };
    command.CommandType = CommandType.Text;
    SqlDataReader dataReader =
        command.ExecuteReader(CommandBehavior.CloseConnection);
    while (dataReader.Read())
    {
        inventory.Add(new CarViewModel
        {
            Id = (int)dataReader["Id"],
            Color = (string)dataReader["Color"],
            Make = (string)dataReader["Make"],
            PetName = (string)dataReader["PetName"]
        });
    }
    dataReader.Close();
    return inventory;
}
```

The next selection method gets a single CarViewModel based on the CarId.

```
public CarViewModel GetCar(int id)
{
    OpenConnection();
    CarViewModel car = null;
    //This should use parameters for security reasons
    string sql =
        $@"SELECT i.Id, i.Color, i.PetName,m.Name as Make
            FROM Inventory i
            INNER JOIN Makes m on m.Id = i.MakeId
            WHERE i.Id = {id}";
    using SqlCommand command =
        new SqlCommand(sql, _sqlConnection)
    {
        CommandType = CommandType.Text
    };
    SqlDataReader dataReader =
        command.ExecuteReader(CommandBehavior.CloseConnection);
```