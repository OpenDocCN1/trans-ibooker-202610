We also have the following classes already implemented in The Art Gallery Simulator project:

| Class Name | Description |
|---|---|
| Person | The class that represents a visitor to the Art Gallery. |
| Persons | The thread-safe storage which keeps objects of the Person class. |
| PersonGenerator | The class is responsible for creating new objects of the Person class at different intervals. This simulates new visitors to the Art Gallery. It is also responsible for the simulation of the situation when people leave the Art Gallery. For this purpose, it triggers remove events at different intervals. |
| Watchman | The class that is responsible for controlling entrance to the Art Gallery. With every new visitor, it checks the number of people in the Art Gallery and if the number is less than 50, it puts a new Person into the list of persons inside the Gallery. If this number is more than the limit, it puts the visitor into the waiting list. It also removes visitors from the list of Persons that are inside the Gallery by using the remove event, which is triggered by the PersonGenerator class. After every removal, it checks whether there are any visitors on the waiting list. If so, it checks the number of people in the Art Gallery and if the number is less than 50, it puts a Person into the lists of persons inside the Gallery. |

Figure 6.21: The table of already implemented classes in The Art Gallery Simulator project