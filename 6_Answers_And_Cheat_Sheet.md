# Answers & Exam-Day Cheat Sheet

---

## Part 1  Answers to File 4 (Practice & Concept-Testing Questions)

**Q1.1.** `new int[2][15]` creates a *rectangular* array  both rows are forced to have 15
columns. It cannot give Row A 10 and Row B 15 different lengths. You need a jagged array instead:
```java
int[][] parkingSlots = new int[2][];
parkingSlots[0] = new int[10];
parkingSlots[1] = new int[15];
```

**Q1.2.**
```java
int[][] rows = new int[3][];
rows[0] = new int[5];
rows[1] = new int[8];
rows[2] = new int[12];
```

**Q1.3.** It returns the number of slots in row index 2 (the 3rd row)  an `int`.

**Q1.4.** Output: `3 6 9 ` (each number followed by a space).

---

**Q2.1.**
```java
if (row < 0 || row > 4) {
    System.out.println("Invalid row number.");
}
```
(Row "cannot be larger than 5" in 1-based terms becomes `row > 4` after the `-1` adjustment,
since 1-based row 5 → 0-based index 4.)

**Q2.2.**
```java
private static void parkCar() {
    Scanner input = new Scanner(System.in);
    int row = input.nextInt() - 1;
    int slot = input.nextInt() - 1;

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
}
```

**Q2.3.** **False.** A single number `a` can never be both less than 0 AND greater than 10 at
the same time, so this condition is always `false` and never fires. To detect "out of range
0–10" you need `||` (OR), not `&&` (AND): `if (a < 0 || a > 10)`.

---

**Q3.1.**
```java
public class Driver {
    private String name;
    private int age;

    public Driver(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }

    public void printDriver() {
        System.out.println("Name: " + name + ", Age: " + age);
    }
}
```

**Q3.2.** `private` hides the field from outside classes  it can only be accessed via getters
and setters, protecting the data (encapsulation). `public` would let any other class change it
directly, bypassing any checks you might add in a setter. The exam expects `private` fields
because encapsulation is a core OOP concept being assessed (LO3).

**Q3.3.**
```java
Driver d = new Driver("Alex", 30);
```

**Q3.4.** Bug: `name = name;` and `age = age;` assign the **parameter to itself**  the field
never actually gets updated (it stays `null`/`0`). It should be `this.name = name;` and
`this.age = age;` to correctly refer to the object's field.

---

**Q4.1.**
```java
private static Driver[] drivers = new Driver[50];
private static int driverCount = 0;
```

**Q4.2.**
```java
Driver d = new Driver("Sam", 25);
drivers[driverCount] = d;
driverCount++;
```

**Q4.3.** The next object added would **overwrite** the previous one at the same index (since
`driverCount` never advances), and searches would miss earlier stored drivers  data would
silently be lost.

**Q4.4.** If you create the object before checking success, you'd end up storing a record for an
action that never actually happened (e.g. a car that wasn't actually parked because the slot was
taken)  the data would be wrong/inconsistent with reality.

---

**Q5.1.**
```java
private static void searchDrivers(int targetAge) {
    boolean found = false;
    for (int i = 0; i < driverCount; i++) {
        if (drivers[i].getAge() == targetAge) {
            System.out.println(drivers[i].getName());
            found = true;
        }
    }
    if (!found) {
        System.out.println("No drivers found.");
    }
}
```

**Q5.2.** Only 10 of the 50 slots actually contain real `Driver` objects; the other 40 are still
`null`. Calling `.getAge()` on a `null` element throws a `NullPointerException` and crashes the
program.

**Q5.3.** Use `.equals()` (or `.equalsIgnoreCase()` for case-insensitive comparison), e.g.
`str1.equals(str2)`. Never use `==` for String content comparison.

---

**Q6.1.**
```java
import java.io.FileWriter;
import java.io.IOException;
```

**Q6.2.**
```java
private static void saveDriversToFile() {
    try {
        FileWriter writer = new FileWriter("drivers.txt");
        for (int i = 0; i < driverCount; i++) {
            writer.write(drivers[i].getName() + "," + drivers[i].getAge());
            writer.write(System.lineSeparator());
        }
        writer.close();
        System.out.println("Saved successfully.");
    } catch (IOException e) {
        System.out.println("An error occurred while saving.");
    }
}
```

**Q6.3.** Because `FileWriter` operations can throw a **checked exception** (`IOException`) 
Java requires checked exceptions to be either caught or declared with `throws`, and `try/catch`
is the standard way to handle it gracefully instead of crashing.

**Q6.4.** The data may not be fully written to disk (it can remain buffered in memory), and the
file may end up incomplete, empty, or locked for other programs to use.

---

**Rapid Fire answers:**
1. `7`
2. `19`
3. `if (age == 18)`  use `==` for comparison, `=` is assignment
4. `private`
5. The current object instance being constructed/operated on
6. `do-while`
7. `'C'`
8. `ArrayIndexOutOfBoundsException`
9. `IOException` (or `FileNotFoundException` depending on the writer used)
10. `true`

---

## Part 2  Answers to File 5 (Full Practice Lab Test)

### Answer 1
```java
public static void initialiseParkingSlots() {
    parkingSlots = new int[4][];
    parkingSlots[0] = new int[12]; // Row A
    parkingSlots[1] = new int[24]; // Row B
    parkingSlots[2] = new int[24]; // Row C
    parkingSlots[3] = new int[12]; // Row D
}
```

### Answer 2
```java
private static void parkCar() {
    Scanner input = new Scanner(System.in);

    System.out.print("Enter parking row (1-4): ");
    int row = input.nextInt() - 1;

    System.out.print("Enter parking slot: ");
    int slot = input.nextInt() - 1;

    if (row < 0 || row > 3) {
        System.out.println("Invalid row number.");
    } else if (slot < 0 || slot >= parkingSlots[row].length) {
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

### Answer 3
```java
public class Vehicle {

    private String registrationNumber;
    private double parkingFee;

    public Vehicle(String registrationNumber, double parkingFee) {
        this.registrationNumber = registrationNumber;
        this.parkingFee = parkingFee;
    }

    public String getRegistrationNumber() {
        return registrationNumber;
    }

    public void setRegistrationNumber(String registrationNumber) {
        this.registrationNumber = registrationNumber;
    }

    public double getParkingFee() {
        return parkingFee;
    }

    public void setParkingFee(double parkingFee) {
        this.parkingFee = parkingFee;
    }

    public void printVehicle() {
        System.out.println("Registration: " + registrationNumber + ", Fee: £" + parkingFee);
    }
}
```

### Answer 4
```java
// 1. Code for creating Vehicle object array
private static Vehicle[] vehicles = new Vehicle[100];
private static int vehicleCount = 0;

// 2. Code for updated parkCar() method
private static void parkCar() {
    Scanner input = new Scanner(System.in);

    System.out.print("Enter registration number: ");
    String reg = input.nextLine();

    System.out.print("Enter parking row (1-4): ");
    int row = input.nextInt() - 1;

    System.out.print("Enter parking slot: ");
    int slot = input.nextInt() - 1;

    if (row < 0 || row > 3) {
        System.out.println("Invalid row number.");
    } else if (slot < 0 || slot >= parkingSlots[row].length) {
        System.out.println("Invalid slot number.");
    } else if (parkingSlots[row][slot] == 0) {
        parkingSlots[row][slot] = 1;

        double fee;
        if (row == 0 || row == 3) {
            fee = 4;
        } else {
            fee = 7;
        }

        Vehicle vehicle = new Vehicle(reg, fee);
        vehicles[vehicleCount] = vehicle;
        vehicleCount++;

        System.out.println("Vehicle parked successfully. Fee: £" + fee);
        showParkingLayout();
    } else {
        System.out.println("This parking slot is already occupied.");
    }
}
```
*(An equally valid alternative for the fee calculation is a `pricePerRow[]` array declared as a
global variable, then `fee = pricePerRow[row];`  either approach is correct.)*

### Answer 5
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
        System.out.println("No vehicles were found for that amount.");
    }
}
```

### Answer 6
```java
private static void saveToFile() {
    try {
        FileWriter writer = new FileWriter("vehicles.txt");

        for (int i = 0; i < vehicleCount; i++) {
            writer.write(vehicles[i].getRegistrationNumber() + "," + vehicles[i].getParkingFee());
            writer.write(System.lineSeparator());
        }

        writer.close();
        System.out.println("Saved successfully.");

    } catch (IOException e) {
        System.out.println("An error occurred while saving.");
    }
}
```
*(Remember `import java.io.FileWriter;` and `import java.io.IOException;` at the top of the file
if the question asks for fully complete code including imports.)*

---

## Part 3  Exam-Day Cheat Sheet (mental checklist, ~2 min review before you start)

**Before writing anything, ask yourself:**
- [ ] Is this a full-method rewrite, or just an addition? (Re-read the question wording.)
- [ ] Are indexes 0-based or 1-based here? Did the given code already do `- 1`?
- [ ] Does my validation use `||` (OR) for "out of range," never `&&` (AND)?
- [ ] Am I comparing Strings with `.equals()`, not `==`?
- [ ] Did I use `this.field = param` in every constructor/setter?
- [ ] Am I only creating/storing an object *inside* the success branch?
- [ ] Am I looping only up to the actual count, not the array's full capacity?
- [ ] Did I import `java.io.FileWriter` / `java.io.IOException` if writing file code?
- [ ] Did I wrap file I/O in `try/catch`?
- [ ] Did I close the writer?

**Common Java syntax to have memorised cold:**
```java
// Class skeleton
public class ClassName {
    private Type field1;
    private Type field2;

    public ClassName(Type field1, Type field2) {
        this.field1 = field1;
        this.field2 = field2;
    }

    public Type getField1() { return field1; }
    public void setField1(Type field1) { this.field1 = field1; }

    public void printSomething() {
        System.out.println(field1 + " " + field2);
    }
}

// Array of objects
private static ClassName[] items = new ClassName[100];
private static int itemCount = 0;

// Search pattern
boolean found = false;
for (int i = 0; i < itemCount; i++) {
    if (items[i].getSomeField() == target) {
        System.out.println(items[i].getOtherField());
        found = true;
    }
}
if (!found) System.out.println("Not found message.");

// File save pattern
try {
    FileWriter writer = new FileWriter("filename.txt");
    for (int i = 0; i < itemCount; i++) {
        writer.write(items[i].getSomeField() + "," + items[i].getOtherField());
        writer.write(System.lineSeparator());
    }
    writer.close();
} catch (IOException e) {
    System.out.println("An error occurred while saving.");
}
```

Good luck  you've got this. Master the **pattern**, and any variation of numbers/names/field
labels on the day will just be filling in the same shape you've already practiced.
