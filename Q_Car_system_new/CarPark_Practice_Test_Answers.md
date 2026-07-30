# Answers — Car Park Practice Test

## Task 1
```java
public static void initialiseParkingSlots() {
    parkingSlots = new int[5][];
    parkingSlots[0] = new int[18]; // Row A
    parkingSlots[1] = new int[20]; // Row B
    parkingSlots[2] = new int[20]; // Row C
    parkingSlots[3] = new int[18]; // Row D
    parkingSlots[4] = new int[14]; // Row E
}
```
The outer array had to grow from `new int[4][]` to `new int[5][]` before the new row's assignment
line could validly be added — this is the step most commonly forgotten.

## Task 2
```java
private static void parkCar() {
    Scanner input = new Scanner(System.in);
    System.out.print("Enter parking row (1-4): ");
    int row = input.nextInt() - 1;

    System.out.print("Enter parking slot: ");
    int slot;
    try {
        slot = input.nextInt();
    } catch (Exception e) {
        System.out.println("Invalid input. Slot number must be an integer.");
        return;
    }
    slot = slot - 1;

    if (slot < 0 || slot >= parkingSlots[row].length) {
        System.out.println("Invalid slot number.");
    } else if (parkingSlots[row][slot] == 0) {
        parkingSlots[row][slot] = 1;
        System.out.println("Vehicle parked successfully.");
        showParkingLayout();
    } else {
        System.out.println("This parking slot is already occupied.");
    }
}
```
`parkingSlots[row].length` automatically gives the correct maximum for whichever row was picked
— 18, 20, or now 14 for Row E — with no hardcoded numbers anywhere. (This example doesn't add
row-number validation, since Task 2 only asked about the slot number — if your real exam also
asks for row validation, add `if (row < 0 || row > 4)` as the first check, updating the `4` to
match however many rows currently exist, or better yet use `parkingSlots.length - 1` if you want
to avoid hardcoding that too.)

## Task 3
```java
public class Attendant {

    private String firstName;
    private String lastName;
    private int attendantID;

    public Attendant(String firstName, String lastName, int attendantID) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.attendantID = attendantID;
    }

    public String getFirstName() {
        return firstName;
    }
    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    public String getLastName() {
        return lastName;
    }
    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public int getID() {
        return attendantID;
    }
    public void setID(int attendantID) {
        this.attendantID = attendantID;
    }

    public void printAttendant() {
        System.out.println("Name: " + firstName + " " + lastName + ", Attendant ID: " + attendantID);
    }

    // Encapsulation has been applied by declaring all attributes (firstName, lastName,
    // attendantID) as private, preventing direct access from outside this class. All access to
    // this data is instead controlled through the public getter and setter methods.
}
```

## Task 4
```java
private static Attendant[] attendants = new Attendant[100];
```

## Task 5
```java
private static void addAttendant() {
    Scanner input = new Scanner(System.in);

    System.out.print("Enter first name: ");
    String firstName = input.nextLine();

    System.out.print("Enter last name: ");
    String lastName = input.nextLine();

    System.out.print("Enter attendant ID: ");
    int attendantID = input.nextInt();

    Attendant attendant = new Attendant(firstName, lastName, attendantID);

    boolean added = false;

    for (int i = 0; i < attendants.length; i++) {
        if (attendants[i] == null) {
            attendants[i] = attendant;
            added = true;
            break;
        }
    }

    if (added) {
        System.out.println("Attendant added successfully.");
    } else {
        System.out.println("Attendant list is full. Cannot add more attendants.");
    }
}
```

## Task 6
Single new line for `getOption()` (option 3, since 1 and 2 are already used):
```java
System.out.println("| 3) Add attendant           |");
```

Complete `runMenu()`:
```java
public static void runMenu() {
    int option;
    boolean cont = true;

    while (cont) {
        option = getOption();
        switch (option) {
            case 0:
                cont = false;
                break;
            case 1:
                parkCar();
                break;
            case 2:
                showParkingLayout();
                break;
            case 3:
                addAttendant();
                break;
            default:
                System.out.println("Option not available. Please select a valid option.");
        }
    }
    System.out.println("Thank you for using Smart Car Park.");
}
```
Note: the original real `runMenu()` has a `System.out.println("Thank you for using Smart Car
Park.");` line after the `while` loop ends — don't drop it when rewriting the "complete method,"
since the real given code included it.

---

## Self-Grading Checklist
- [ ] Task 1: Resized the **outer** array to `new int[5][]`, not just added a new row line?
- [ ] Task 2: Used `try/catch` for the integer check, **before** the `-1` conversion and range
      checks?
- [ ] Task 2: Used `parkingSlots[row].length` instead of hardcoding `18`/`20`/`14` anywhere?
- [ ] Task 3: Used the exact specified getter/setter names (`getID()`/`setID()`, not
      `getAttendantID()`/`setAttendantID()`)?
- [ ] Task 3: Included an actual `//` comment at the end explaining encapsulation?
- [ ] Task 4: Avoided `ArrayList`, used a plain array instead?
- [ ] Task 5: Used the null-slot-search pattern (no counter was declared in Task 4)?
- [ ] Task 5: Used `break;` once a free slot was found?
- [ ] Task 6: Noticed option 3 (not 4) was the correct free menu number for **this specific**
      project, rather than assuming it's always 4?
- [ ] Task 6: Kept the original `runMenu()`'s closing "Thank you" message when rewriting the
      "complete method"?

If every box is ticked, you've correctly transferred the pattern to a real, unfamiliar variation
of the task — which is exactly what your actual exam will require.
