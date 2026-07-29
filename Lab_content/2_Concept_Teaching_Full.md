# Master Class: Every Java Concept You Need  From Zero

This file teaches each concept **from scratch**, using small, simple examples first (not the
exam project itself  that comes in file 3), so you build real understanding, not memorisation.

---

## 1. Arrays (1D, 2D, and Jagged Arrays)

### 1D Arrays
```java
int[] numbers = new int[5];   // 5 slots, indexes 0-4, all default to 0
numbers[0] = 10;
System.out.println(numbers[0]); // 10
System.out.println(numbers.length); // 5 (a property, not a method  no brackets!)
```

### 2D Arrays (rectangular)
```java
int[][] grid = new int[3][4]; // 3 rows, each with exactly 4 columns
grid[0][0] = 1;
grid[2][3] = 9;
```

### Jagged Arrays (rows of *different* lengths)  **this is what the Car Park project uses**
```java
int[][] parkingSlots = new int[4][];  // 4 rows, but lengths not yet fixed
parkingSlots[0] = new int[18];        // Row A has 18 slots
parkingSlots[1] = new int[20];        // Row B has 20 slots
parkingSlots[2] = new int[20];        // Row C has 20 slots
parkingSlots[3] = new int[18];        // Row D has 18 slots
```
**Why jagged, not rectangular?** Because each row can hold a different number of items. If you
wrote `new int[4][20]`, every row would be forced to have 20 columns  not flexible enough.

**Key exam skill:** if asked to change slot counts per row, you only touch the *numbers inside
the brackets* on each row's line  the structure (`new int[4][]`, then 4 separate assignment
lines) stays the same.

```java
public static void initialiseParkingSlots() {
    parkingSlots = new int[4][];
    parkingSlots[0] = new int[16];  // changed
    parkingSlots[1] = new int[22];  // changed
    parkingSlots[2] = new int[22];  // changed
    parkingSlots[3] = new int[16];  // changed
}
```

### Looping through 2D/jagged arrays
```java
for (int row = 0; row < parkingSlots.length; row++) {
    for (int slot = 0; slot < parkingSlots[row].length; slot++) {
        System.out.print(parkingSlots[row][slot]);
    }
}
```
Notice: `parkingSlots[row].length` gives the length of **that specific row**, which is why this
works even though rows have different lengths.

---

## 2. Input Validation

The existing pattern in the project only checks **one** condition (is the slot occupied?). The
exam usually asks you to **add more checks before that**, typically range checks.

### Basic structure
```java
if (row < 0 || row > 3) {
    System.out.println("Invalid row number.");
} else if (slot < 0 || slot >= parkingSlots[row].length) {
    System.out.println("Invalid slot number.");
} else if (parkingSlots[row][slot] == 0) {
    parkingSlots[row][slot] = 1;
    System.out.println("Vehicle parked successfully.");
} else {
    System.out.println("This parking slot is already occupied.");
}
```

**Critical detail:** remember the input is often converted from 1-based (what the user types) to
0-based (array index) *before* validation, e.g. `int row = input.nextInt() - 1;`. So:
- "Row cannot be smaller than 1" → in 0-based terms this becomes `row < 0`
- "Row cannot be larger than 4" → in 0-based terms this becomes `row > 3` (since index 3 = the 4th row)

Always double check **which variable (raw input or the -1 adjusted variable) you are validating**,
and match your comparison numbers to that.

### Re-prompting loops (defensive technique, useful if asked for it)
```java
int row;
do {
    System.out.print("Enter parking row (1-4): ");
    row = input.nextInt() - 1;
    if (row < 0 || row > 3) {
        System.out.println("Invalid row. Try again.");
    }
} while (row < 0 || row > 3);
```

---

## 3. Object-Oriented Programming  Classes

### The blueprint idea
A **class** is a template. An **object** is a real instance created from that template.

```java
public class Vehicle {

    // 1. Fields (attributes)  private = encapsulation
    private String registrationNumber;
    private double parkingFee;

    // 2. Constructor  runs when you create a new object with `new`
    public Vehicle(String registrationNumber, double parkingFee) {
        this.registrationNumber = registrationNumber;
        this.parkingFee = parkingFee;
    }

    // 3. Getters  return the current value of a field
    public String getRegistrationNumber() {
        return registrationNumber;
    }

    public double getParkingFee() {
        return parkingFee;
    }

    // 4. Setters  change the value of a field
    public void setRegistrationNumber(String registrationNumber) {
        this.registrationNumber = registrationNumber;
    }

    public void setParkingFee(double parkingFee) {
        this.parkingFee = parkingFee;
    }

    // 5. A method that prints information about this object
    public void printVehicle() {
        System.out.println("Registration: " + registrationNumber + ", Fee: £" + parkingFee);
    }
}
```

**Why `this.registrationNumber = registrationNumber`?**
The parameter and the field have the same name on purpose (this is normal, expected style).
`this.registrationNumber` means "the field belonging to this object," while `registrationNumber`
alone (on the right) means "the parameter that was just passed in." Without `this.`, Java can't
tell them apart and the assignment would do nothing useful.

**Creating an object:**
```java
Vehicle v = new Vehicle("AB12 CDE", 50.0);
v.printVehicle(); // Registration: AB12 CDE, Fee: £50.0
System.out.println(v.getParkingFee()); // 50.0
v.setParkingFee(80.0);
```

---

## 4. Arrays of Objects + Conditional Creation

### Declaring an array of objects (as a global variable)
```java
private static Vehicle[] vehicles = new Vehicle[100];
private static int vehicleCount = 0; // tracks how many are actually filled in
```
This creates 100 **empty slots** (each `null` until you put a real object there). You are
responsible for tracking how many are actually used  that's what `vehicleCount` is for.

### Only creating an object if a condition succeeds
This is the pattern behind "Question 4" style tasks: you must gather input, calculate something,
check a condition, and only THEN build the object.

```java
private static void parkCar() {
    Scanner input = new Scanner(System.in);

    System.out.print("Enter registration number: ");
    String reg = input.nextLine();

    System.out.print("Enter parking row (1-4): ");
    int row = input.nextInt() - 1;

    System.out.print("Enter parking slot: ");
    int slot = input.nextInt() - 1;

    if (row < 0 || row > 3 || slot < 0 || slot >= parkingSlots[row].length) {
        System.out.println("Invalid row or slot.");
        return; // stop here  nothing else should run
    }

    if (parkingSlots[row][slot] == 0) {
        parkingSlots[row][slot] = 1;

        double fee = pricePerRow[row]; // calculation happens BEFORE object creation

        Vehicle v = new Vehicle(reg, fee);
        vehicles[vehicleCount] = v;
        vehicleCount++;

        System.out.println("Vehicle parked successfully. Fee: £" + fee);
        showParkingLayout();
    } else {
        System.out.println("This parking slot is already occupied.");
    }
}
```

**Exam-critical order to remember:**
1. Collect all required input (including any *new* field, like a registration number/email 
   collected **first**, before row/slot, if the question says so)
2. Validate ranges
3. Check availability
4. **Only if successful:** calculate the fee/price
5. **Only if successful:** create the object and store it in the array
6. Increment your counter

A common mistake is creating the object *before* checking whether the slot was actually free 
always create it last, only inside the "success" branch.

---

## 5. Searching an Array of Objects

The pattern: loop through the *used* portion of the array, compare a field, track whether
anything matched.

```java
private static void searchVehicles() {
    Scanner input = new Scanner(System.in);
    System.out.print("Enter fee amount to search for: ");
    double amount = input.nextDouble();

    boolean found = false;

    for (int i = 0; i < vehicleCount; i++) {
        if (vehicles[i].getParkingFee() == amount) {
            System.out.println(vehicles[i].getRegistrationNumber());
            found = true;
        }
    }

    if (!found) {
        System.out.println("No payments were found.");
    }
}
```

**Key things examiners look for:**
- Looping only up to `vehicleCount` (or however many objects actually exist), **not** the full
  array capacity (100)  otherwise you'll hit `null` objects and crash with a
  `NullPointerException`.
- Using a `boolean found` flag, set to `true` inside the loop, checked *after* the loop ends.
- Comparing `double`/`int` fields with `==`, but comparing `String` fields with `.equals()`
  (never `==` for Strings!).

```java
// if searching by a String field instead (e.g. registration number):
if (vehicles[i].getRegistrationNumber().equals(searchValue)) { ... }
```

---

## 6. File I/O  Saving to a Text File

### The simplest reliable pattern using `FileWriter`
```java
import java.io.FileWriter;
import java.io.IOException;

private static void saveToFile() {
    try {
        FileWriter writer = new FileWriter("vehicles.txt");

        for (int i = 0; i < vehicleCount; i++) {
            writer.write(vehicles[i].getRegistrationNumber() + "," + vehicles[i].getParkingFee());
            writer.write(System.lineSeparator()); // moves to a new line
        }

        writer.close(); // always close the file when done!
        System.out.println("Saved successfully.");

    } catch (IOException e) {
        System.out.println("An error occurred while saving.");
    }
}
```

### Alternative using `PrintWriter` (slightly cleaner, has `println`)
```java
import java.io.PrintWriter;
import java.io.FileNotFoundException;

private static void saveToFile() {
    try {
        PrintWriter writer = new PrintWriter("vehicles.txt");

        for (int i = 0; i < vehicleCount; i++) {
            writer.println(vehicles[i].getRegistrationNumber() + "," + vehicles[i].getParkingFee());
        }

        writer.close();
        System.out.println("Saved successfully.");

    } catch (FileNotFoundException e) {
        System.out.println("An error occurred while saving.");
    }
}
```

**Remember for the exam:**
- File writing operations **must** be wrapped in `try/catch` because Java forces you to handle
  the possible checked exception (`IOException` or `FileNotFoundException`).
- Import statements matter  if asked to write "complete" code, include the `import` line(s).
- Always `close()` the writer (or examiners may deduct marks even if it's not always enforced
  by the compiler in a snippet).
- "One line per record" means one `write`/`println` call per loop iteration  don't put a comma
  loop of all records into a single write.

---

## 7. Supporting Fundamentals Refresher

### Scanner gotchas
```java
Scanner input = new Scanner(System.in);
int row = input.nextInt();     // reads a number
input.nextLine();              // ⚠ consumes the leftover newline character
String email = input.nextLine(); // now this actually works correctly
```
If you mix `nextInt()`/`nextDouble()` with `nextLine()` without this extra consuming line, the
`nextLine()` call will appear to be "skipped"  a classic bug. In the actual project, notice
`parkCar()` only ever uses `nextInt()`, so this isn't an issue *yet*  but the moment you add
`input.nextLine()` for an email/registration string, watch out for this.

### `switch` statement (used in `runMenu()`)
```java
switch (option) {
    case 0:
        cont = false;
        break;
    case 1:
        parkCar();
        break;
    default:
        System.out.println("Option not available.");
}
```
Always include `break;` unless you deliberately want fall-through, and always include a
`default` case.

### static vs instance
- Everything in `App.java` is `static` because `main()` is static, and static methods can only
  directly call other static methods/variables without creating an object first.
- Your new class (e.g. `Ticket`/`Vehicle`/`Payment`) should generally **NOT** be static  its
  methods are called on objects (`v.printVehicle()`), which is normal OOP style.

### char arithmetic (used in `showParkingLayout()`)
```java
System.out.print("Row " + (char) ('A' + row) + " ");
```
`'A'` has the numeric value 65. Adding `row` (0,1,2,3) and casting back to `char` gives
`'A','B','C','D'`. Useful if you're asked to label something similarly.

---

## Quick Self-Test (answer before moving to file 3)
1. Why is `parkingSlots` declared as `int[4][]` instead of `int[4][20]`?
2. If a question says "row cannot be larger than 4," and the code does `row = input.nextInt() - 1`,
   what should your upper-bound check actually compare against  `4` or `3`? Why?
3. In a constructor, why do we write `this.field = field` instead of just `field = field`?
4. Why must you loop only up to `vehicleCount` and not the full array length when searching?
5. What Java exception type do you need to catch when using `FileWriter`?

*(Answers are in file 6.)*
