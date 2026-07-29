# Car Park Management  Full Deep Dive

This walks through the **actual provided project** line-by-line, then gives you fully worked
example solutions for all 6 skill areas, styled exactly like the real exam's likely tasks.

> ⚠️ **Important:** The Final Lab Practical will NOT ask the exact same numbers/names as below.
> These are worked *examples* using the same patterns  study the technique, not the literal text.

---

## Part A  Understanding the Existing Code (line-by-line)

### Lines 1–6: Imports and the global variable
```java
import java.util.Scanner;
public class App {
    private static int[][] parkingSlots = null;
```
- `Scanner` is imported for reading user input.
- `parkingSlots` is a **static, private, jagged 2D array**  static because `main()` needs to
  access it without creating an `App` object; private because it's only used inside this class.

### Lines 8–16: `main()`
```java
public static void main(String[] args) {
    System.out.println("Welcome to Smart Car Park!");
    initialiseParkingSlots();
    runMenu();
}
```
Three jobs only: greet the user, set up the data, start the menu loop. This method rarely needs
editing itself  the exam edits the methods it *calls*.

### Lines 19–35: `initialiseParkingSlots()`
```java
public static void initialiseParkingSlots() {
    parkingSlots = new int[4][];
    parkingSlots[0] = new int[18]; // Row A
    parkingSlots[1] = new int[20]; // Row B
    parkingSlots[2] = new int[20]; // Row C
    parkingSlots[3] = new int[18]; // Row D
}
```
This is your **"change the number of seats/slots per row"** target method. It always has this
exact shape: create the outer array with 4 empty rows, then assign each row's own length.

### Lines 38–70: `runMenu()`
```java
public static void runMenu() {
    int option;
    boolean cont = true;
    while (cont) {
        option = getOption();
        switch (option) {
            case 0: cont = false; break;
            case 1: parkCar(); break;
            case 2: showParkingLayout(); break;
            default: System.out.println("Option not available. Please select a valid option.");
        }
    }
    System.out.println("Thank you for using Smart Car Park.");
}
```
Standard "menu loop" pattern: keep looping until the user picks `0`. If you're ever asked to add
a **new menu option** (e.g. "3) Search vehicles" or "4) Save to file"), this is the method you'd
extend  add a new `case` and call your new method.

### Lines 73–113: `getOption()`
Reads the option safely, catching bad (non-integer) input so the program doesn't crash.
```java
try {
    option = input.nextInt();
    valid = true;
} catch (Exception e) {
    System.out.println("This option is not valid.");
    input.nextLine();
}
```
Notice `input.nextLine()` inside the catch  this clears the bad input left in the buffer so the
loop doesn't spin forever on the same invalid token.

### Lines 116–140: `parkCar()`   **this is your `buyTicket()` equivalent**
```java
private static void parkCar() {
    Scanner input = new Scanner(System.in);
    System.out.print("Enter parking row (1-4): ");
    int row = input.nextInt() - 1;
    System.out.print("Enter parking slot: ");
    int slot = input.nextInt() - 1;

    if (parkingSlots[row][slot] == 0) {
        parkingSlots[row][slot] = 1;
        System.out.println("Vehicle parked successfully.");
        showParkingLayout();
    } else {
        System.out.println("This parking slot is already occupied.");
    }
}
```
This is the **most important method in the whole project**  nearly every exam question
(validation, new class, arrays of objects, pricing, search source data) revolves around
extending this method. Note it currently has **no bounds checking at all**  if you type row 9,
it will crash with an `ArrayIndexOutOfBoundsException`. That's exactly the gap Question-2-style
tasks ask you to fix.

### Lines 143–188: `showParkingLayout()`
```java
private static void showParkingLayout() {
    int rows = parkingSlots.length;
    char lane = '|';
    ...
    for (int row = 0; row < rows; row++) {
        System.out.print("Row " + (char) ('A' + row) + " ");
        int slots = parkingSlots[row].length;
        for (int slot = 0; slot < slots; slot++) {
            if (slot == 9) System.out.print(" " + lane + " ");
            if (parkingSlots[row][slot] == 0) System.out.print("[O]");
            else System.out.print("[X]");
        }
        System.out.println();
    }
    ...
}
```
Displays every row, labelling rows `A`, `B`, `C`, `D` using char arithmetic, and every slot as
`[O]` (available) or `[X]` (occupied), with a visual "driving lane" inserted after the 9th slot
in each row.

---

## Part B  Worked Solutions for Each Skill Area

### 🅰️ Skill 1: Changing slots per row (rewrite `initialiseParkingSlots()`)

**Example task:** "Row A should have 15 slots, Row B 25, Row C 25, Row D 15."

```java
public static void initialiseParkingSlots() {
    parkingSlots = new int[4][];
    parkingSlots[0] = new int[15]; // Row A
    parkingSlots[1] = new int[25]; // Row B
    parkingSlots[2] = new int[25]; // Row C
    parkingSlots[3] = new int[15]; // Row D
}
```
Only the numbers in the brackets change  the method signature, the outer array declaration,
and the number of rows (4) stay exactly the same unless the task explicitly says otherwise.

---

### 🅱️ Skill 2: Adding validation to `parkCar()`

**Example task:** "Row number cannot be smaller than 1 or larger than 4. Slot number cannot be
smaller than 1 or larger than the number of slots in that row."

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
Note the whole `if/else if/else if/else` is **one chain**  this ensures only one message is
ever printed per attempt, and later checks (like "already occupied") only run once earlier
checks (like valid row) have passed.

---

### 🅲️ Skill 3: A new class  `Ticket.java`

**Example task:** create a class with a vehicle registration number and a parking fee, plus a
`printTicket()` method.

```java
public class Ticket {

    private String registrationNumber;
    private double parkingFee;

    public Ticket(String registrationNumber, double parkingFee) {
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

    public void printTicket() {
        System.out.println("Registration: " + registrationNumber + ", Fee: £" + parkingFee);
    }
}
```

---

### 🅳️ Skill 4: Array of `Ticket` objects + updated `parkCar()`

**Example task:** "Add a global array that can store 100 Ticket objects. Update `parkCar()` to
ask for the registration number first, then calculate the fee (Row A/D = £5, Row B/C = £8), and
only create a Ticket if the vehicle was parked successfully."

```java
// 1. Global array (add near the top of App.java, with parkingSlots)
private static Ticket[] tickets = new Ticket[100];
private static int ticketCount = 0;
private static double[] pricePerRow = {5, 8, 8, 5};

// 2. Updated parkCar()
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

        double fee = pricePerRow[row];

        Ticket ticket = new Ticket(reg, fee);
        tickets[ticketCount] = ticket;
        ticketCount++;

        System.out.println("Vehicle parked successfully. Fee: £" + fee);
        showParkingLayout();
    } else {
        System.out.println("This parking slot is already occupied.");
    }
}
```
⚠️ Because `getOption()` uses `input.nextInt()` right before this method runs, and this version
now uses `input.nextLine()` to read the registration number, you may need an extra
`input.nextLine();` immediately after `getOption()`'s `nextInt()` call returns, OR create the
`Scanner` carefully  this "leftover newline" issue is a common real gotcha. If your test setup
already avoids this (e.g. the given project handles it), don't add anything unnecessary  but
know **why** it can happen if your output looks like a field was skipped.

---

### 🅴 Skill 5: `searchTickets()`

**Example task:** "Ask the user for a fee amount, print all registration numbers with that fee,
or say no payments were found."

```java
private static void searchTickets() {
    Scanner input = new Scanner(System.in);
    System.out.print("Enter fee amount to search for: ");
    double amount = input.nextDouble();

    boolean found = false;

    for (int i = 0; i < ticketCount; i++) {
        if (tickets[i].getParkingFee() == amount) {
            System.out.println(tickets[i].getRegistrationNumber());
            found = true;
        }
    }

    if (!found) {
        System.out.println("No payments were found.");
    }
}
```

---

### 🅵 Skill 6: `saveToFile()`

**Example task:** "Save every ticket's registration number and fee, one per line, to a text
file."

```java
private static void saveToFile() {
    try {
        FileWriter writer = new FileWriter("tickets.txt");

        for (int i = 0; i < ticketCount; i++) {
            writer.write(tickets[i].getRegistrationNumber() + "," + tickets[i].getParkingFee());
            writer.write(System.lineSeparator());
        }

        writer.close();
        System.out.println("Saved successfully.");

    } catch (IOException e) {
        System.out.println("An error occurred while saving.");
    }
}
```
Don't forget: `import java.io.FileWriter;` and `import java.io.IOException;` at the top of the
file if you're asked to write the "complete" method/imports.

---

## Part C  How These Pieces Fit Together (the full mental model)

```
main()
  → initialiseParkingSlots()   [Skill 1 lives here]
  → runMenu()
       → getOption()
       → parkCar()             [Skills 2 and 4 live here]
       → showParkingLayout()
       → searchTickets()       [Skill 5  a NEW method you add + a NEW menu case]
       → saveToFile()          [Skill 6  a NEW method you add + a NEW menu case]

Ticket.java                    [Skill 3  a completely separate class file]
  → used by parkCar() (creates Ticket objects)
  → used by searchTickets() (reads Ticket objects)
  → used by saveToFile() (reads Ticket objects)
```

Everything connects through the `tickets[]` array: `parkCar()` **writes** to it,
`searchTickets()` and `saveToFile()` **read** from it. Understanding this data flow is often more
valuable on exam day than memorising exact code, because the exam will phrase things slightly
differently every time.

---

## Part D  Things That Commonly Trip Students Up
1. **Off-by-one errors**: remember `- 1` conversions from 1-based user input to 0-based array index.
2. **Forgetting `this.`** in constructors/setters, which silently breaks the assignment.
3. **Looping over the full array capacity (100) instead of the actual count**  causes
   `NullPointerException` when you hit an empty (`null`) slot.
4. **Comparing Strings with `==`** instead of `.equals()`.
5. **Forgetting to import** `java.io.FileWriter` / `java.io.IOException` when writing file code.
6. **Creating the object before checking success**  always calculate/create only inside the
   success branch.
7. **Not rewriting the COMPLETE method** when asked  if the question says "rewrite the complete
   method," include the method signature, braces, and all original working code, not just your
   new lines.
