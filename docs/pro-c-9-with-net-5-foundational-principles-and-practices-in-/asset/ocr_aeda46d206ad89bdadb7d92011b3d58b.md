rather than an ArrayList. Given that Dictionary types allow access to the contained types using a key (such as a person’s first name), you could define an indexer as follows:

```
using System.Collections;
using System.Collections.Generic;
namespace SimpleIndexer
{
    public class PersonCollectionStringIndexer : IEnumerable
    {
        private Dictionary<string, Person> listPeople = new Dictionary<string, Person>();

        // This indexer returns a person based on a string index.
        public Person this[string name]
        {
            get => (Person)listPeople[name];
            set => listPeople[name] = value;
        }
        public void ClearPeople()
        {
            listPeople.Clear();
        }

        public int Count => listPeople.Count;

        IEnumerator IEnumerable.GetEnumerator() => listPeople.GetEnumerator();
    }
}
```

The caller would now be able to interact with the contained Person objects, as shown here:

```
Console.WriteLine("***** Fun with Indexers *****
");

PersonCollectionStringIndexer myPeopleStrings =
    new PersonCollectionStringIndexer();

myPeopleStrings["Homer"] =
    new Person("Homer", "Simpson", 40);
myPeopleStrings["Marge"] =
    new Person("Marge", "Simpson", 38);

// Get "Homer" and print data.
Person homer = myPeopleStrings["Homer"];
Console.ReadLine();
```

Again, if you were to use the generic Dictionary<TKey, TValue> type directly, you’d gain the indexer method functionality out of the box, without building a custom, nongeneric class supporting a string indexer. Nevertheless, do understand that the data type of any indexer will be based on how the supporting collection type allows the caller to retrieve subitems.