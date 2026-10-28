
```java
public int packSize() {
    return cards.size();
}

public List<T> getCards() {
    return new ArrayList<>(cards);
}

protected void setCards(List<T> cards) {
    this.cards = cards;
}
}

public final class StandardPack extends Pack {

    public StandardPack() {
        super.setCards(build());
    }

    @Override
    protected List<StandardCard> build() {

        List<StandardCard> cards = new ArrayList<>();

        // code omitted for brevity

        return cards;
    }
}
```

- Deck of cards implementation provides the following:

```java
public class Deck<T extends Card> implements Iterable<T> {

    private final List<T> cards;

    public Deck(Pack pack) {
```
```
this.cards = pack.getCards();
}

public void shuffle() {...}
public List<T> dealHand(int numberOfCards) {...}
public T dealCard() {...}
public int remainingCards() {...}
public void removeCards(List<T> cards) {...}

@Override
public Iterator<T> iterator() {...}
}
```

A demo of the code can be quickly written as follows:

```
// create a single classical card
Card sevenHeart = new StandardCard(StandardSuit.HEARTS, 7);

// create a complete deck of standards cards
Pack cp = new StandardPack();
Deck deck = new Deck(cp);

System.out.println("Remaining cards: "
    + deck.remainingCards());
```

Furthermore, you can easily add more types of cards by extending the Card and Pack classes. The complete code is named DeckOfCards.

## Example 4: Parking lot

Amazon, Google, Adobe, Microsoft

Problem: Design the main classes of a parking lot.

What to ask: Is it a single-level or multi-level parking lot? Are all parking space spots the same? What type of vehicles should we park? Is it free parking? Do we use parking tickets?

**Interviewer:** It is a synchronous automatic multi-level free parking lot. All parking spots are the same size, but we expect cars (1 spot needed), vans (2 spots needed) and trucks (5 spots needed). Other types of vehicles should be added without modifying the code. The system releases a parking ticket that can be used later to unpark the vehicle. But if the driver introduces only the vehicle information (assuming a lost ticket), the system should still work and locate the vehicle in the parking lot and unpark it.

**Solution:** In order to understand what classes should be involved in our design, we can quickly sketch a parking lot to identify the main actors and behaviors as in Figure 6.4:

![](img/6709a71136a6b327c831c6c15aff2f6a_158_0.png)

Figure 6.4 – A parking lot

The diagram reveals two major actors: the parking lot and the automatic parking system.

First, let's focus on the parking lot. The main purpose of a parking lot is to park vehicles; therefore, we need to shape the accepted vehicles (car, van, and truck). This looks like a typical case for an abstract class (`Vehicle`) and three subclasses (`Car`, `Van`, and `Truck`). But this is not true! The driver provides information about their vehicle. They don't effectively push the vehicle (the object) into the parking system, so our system does not need dedicated objects for cars, vans, trucks, and so on. Think from the perspective of a parking lot. It needs the vehicle license plate and the free spots required for parking. It doesn't care about the characteristics of a van or a truck. So, we can shape a `Vehicle` as follows. Along with the following code, you can use the UML diagram from here: https://github.com/PacktPublishing/The-Complete-Coding-Interview-Guide-in-Java/blob/master/Chapter06/ParkingLot/ParkingLotUML.png

```java
public enum VehicleType {
    CAR(1), VAN(2), TRUCK(5);
```
```
}

public class Vehicle {

    private final String licensePlate;
    private final int spotsNeeded;
    private final VehicleType type;

    public Vehicle(String licensePlate,
                   int spotsNeeded, VehicleType type) {
        this.licensePlate = licensePlate;
        this.spotsNeeded = spotsNeeded;
        this.type = type;
    }

    // getters omitted for brevity

    // equals() and hashCode() omitted for brevity
}
```

Next, we have to design the parking lot. Mainly, a parking lot has several floors (or levels) and each floor has parking spots. Among others, a parking lot should expose methods for parking/unparking a vehicle. These methods will delegate the parking/unparking tasks to each floor (or to a certain floor) until it succeeds or there is no floor to scan:

```java
public class ParkingLot {

    private String name;
    private Map<String, ParkingFloor> floors;

    public ParkingLot(String name) {
        this.name = name;
    }

    public ParkingLot(String name,
                      Map<String, ParkingFloor> floors) {
        this.name = name;
```
```
private int totalFreeSpots;

public ParkingFloor(String name, int totalSpots) {
    this.name = name;
    this.totalSpots = totalSpots;

    initialize(); // create the parking spots
}

protected ParkingTicket parkVehicle(Vehicle vehicle) {...}

//we have to find the vehicle by looping the parking spots
protected boolean unparkVehicle(Vehicle vehicle) {...}

// we have the ticket, so we have the needed information
protected boolean unparkVehicle(
    ParkingTicket parkingTicket) {...}

protected boolean isFull(VehicleType type) {...}
protected int countFreeSpots(
    VehicleType vehicleType) {...}

// getters omitted for brevity

private List<ParkingSpot> findSpotsToFitVehicle(
    Vehicle vehicle){...}
private void assignVehicleToParkingSpots(
    List<ParkingSpot> spots, Vehicle vehicle) {...}
private ParkingTicket releaseParkingTicket(
    Vehicle vehicle) {...}
private ParkingTicket findParkingTicket(
    Vehicle vehicle) {...}
private void registerParkingTicket(
    ParkingTicket parkingTicket) {...}
private boolean unregisterParkingTicket(
    ParkingTicket parkingTicket) {...}
```
```
```java
private void initialize() {...}
}
```

Finally, a parking spot is an object that holds information about its name (label or number), availability (whether it is free) and vehicle (whether a vehicle is parked on that spot). It also has methods for assigning/removing a vehicle to/from this spot:

```java
public class ParkingSpot {

    private boolean free = true;
    private Vehicle vehicle;

    private final String label;
    private final ParkingFloor parkingFloor;

    protected ParkingSpot(ParkingFloor parkingFloor,
                          String label) {
        this.parkingFloor = parkingFloor;
        this.label = label;
    }

    protected boolean assignVehicle(Vehicle vehicle) {...}
    protected boolean removeVehicle() {...}

    // getters omitted for brevity
}
```

At this moment, we have all the major classes of the parking lot. Next, we are going to focus on the automatic parking system. This can be shaped as a single class that acts as a dispatcher of the parking lot:

```java
public class ParkingSystem implements Parking {

    private final String id;
    private final ParkingLot parkingLot;

    public ParkingSystem(String id, ParkingLot parkingLot) {
        this.id = id;
```
```
this.parkingLot = parkingLot;
}

@Override
public ParkingTicket parkVehicleBtn(
    String licensePlate, VehicleType type) {...}

@Override
public boolean unparkVehicleBtn(
    String licensePlate, VehicleType type) {...}

@Override
public boolean unparkVehicleBtn(
    ParkingTicket parkingTicket) {...}

// getters omitted for brevity
}
```

The complete application containing a partial implementation as well is named *ParkingLot*.

## Example 5: Online reader system

**Problem:** Design the main classes of an online reader system.

**What to ask:** What are the required functionalities? How many books can be read simultaneously?

**Interviewer:** The system should be capable of managing readers and books. Your code should be able to add/remove a reader/book and to display a reader/book. The system can serve a single reader and a single book at a time.
