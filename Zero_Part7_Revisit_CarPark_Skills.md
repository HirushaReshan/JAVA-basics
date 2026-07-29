# Java From Absolute Zero — Part 7: The 6 Exam Skills, Fully Re-Taught

Now that you understand every fundamental (Parts 1–6), let's go back through the exact 6 skill
areas of your exam, this time explaining **every single word** of the actual code, assuming
zero prior knowledge — nothing skipped.

---

## Skill 1 — Changing the number of slots per row

```java
public static void initialiseParkingSlots() {
    parkingSlots = new int[4][];
    parkingSlots[0] = new int[18];
    parkingSlots[1] = new int[20];
    parkingSlots[2] = new int[20];
    parkingSlots[3] = new int[18];
}
```

- `public static void initialiseParkingSlots()` — a method. `public` = anyone can call it.
  `static` = belongs to the class, callable without creating an `App` object (needed since
  `main()`, itself static, calls it). `void` = it doesn't hand back any value; its whole job is
  just to set up data as a **side effect**. `initialiseParkingSlots` = the name, describing what
  it does. `()` = it needs no input to do its job.
- `parkingSlots = new int[4][];` — `parkingSlots` is the class's field (declared elsewhere as
  `private static int[][] parkingSlots = null;`). This line **replaces** whatever it held before
  (starts as `null`, meaning "nothing/empty") with a brand new jagged array: 4 outer slots, each
  ready to eventually hold its own inner `int[]` array, but not yet decided how long.
- `parkingSlots[0] = new int[18];` — takes the **first** outer slot (index 0 — remember, always
  zero-indexed) and puts a real, brand-new array of 18 integers into it. All 18 values inside
  default to `0`. In this project, `0` means "available," `1` means "occupied."
- The same pattern repeats for indexes 1, 2, 3 with 20, 20, 18 respectively.

**To change the requirement (e.g. Row A=12, B=24, C=24, D=12), you only change the numbers
inside the square brackets on the four assignment lines** — nothing else about this method's
shape changes.

---

## Skill 2 — Validating row/slot numbers in `parkCar()`

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

- `private static void parkCar()` — `private` because only this class ever needs to call it
  (triggered from the menu). `static` and `void` for the same reasons as before.
- `Scanner input = new Scanner(System.in);` — creates a new `Scanner` **object** (an actual
  instance of Java's built-in `Scanner` class) that watches the keyboard (`System.in`), and
  stores a reference to it in the local variable `input`.
- `System.out.print("Enter parking row (1-4): ");` — note `print`, not `println` — prints the
  text **without** moving to a new line afterward, so the user's typed answer appears right next
  to the prompt on the same line.
- `int row = input.nextInt() - 1;` — `input.nextInt()` pauses the program and waits for the user
  to type a whole number and press Enter, then hands that number back. `- 1` immediately
  converts it from the human-friendly "1 to 4" numbering into the computer's zero-indexed "0 to
  3" numbering, and the *result* of that subtraction is what actually gets stored into `row`.
- The exact same pattern happens for `slot`.
- `if (row < 0 || row > 3)` — reads as: "if row is less than 0, **OR** row is greater than 3."
  Since valid indexes are 0,1,2,3 (four rows total), anything outside that range is invalid. `||`
  means only ONE of these needs to be true for the whole condition to be true.
- `System.out.println("Invalid row number.");` — runs only if the row was invalid; the method
  effectively stops meaningfully here (the rest of the `if/else if` chain is skipped entirely
  because only one branch of a chain ever runs).
- `else if (slot < 0 || slot >= parkingSlots[row].length)` — this only gets **checked** if the
  row turned out to be valid (because it's an `else if`). It checks the slot is within range:
  not negative, and not equal to or beyond the actual length of *that specific row's* array
  (`parkingSlots[row].length` — remember from Part 4, this gets the length of just that one
  row, which might be 18 or 20 depending which row was chosen).
- `else if (parkingSlots[row][slot] == 0)` — only checked if BOTH the row and slot were valid.
  Looks up the actual value stored at that exact row/slot position. `0` means available.
- `parkingSlots[row][slot] = 1;` — marks that slot as now occupied.
- `showParkingLayout();` — calls another method (defined elsewhere in the class) to display the
  updated layout to the user immediately.
- `else { ... "already occupied." }` — the final catch-all: reached only if row was valid, slot
  was valid, but the slot's value was **not** `0` (so it must already be `1`, occupied).

**Why is order important here?** If you tried to check `parkingSlots[row][slot] == 0` *before*
checking that `row` and `slot` are in valid ranges, and the user typed an invalid row like `9`,
Java would try to access `parkingSlots[9]` — which doesn't exist — and crash with an
`ArrayIndexOutOfBoundsException` before ever reaching your friendly "invalid" message. **Always
validate ranges first**, before using those values to access an array.

---

## Skill 3 — A brand new class, e.g. `Ticket.java`

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

- `public class Ticket {` — declares a brand new class, called `Ticket`, publicly usable by any
  other class (specifically, `App.java` needs to use it). This lives in its **own file**,
  `Ticket.java`, because in Java, a `public` class's name must match its file's name exactly.
- `private String registrationNumber;` and `private double parkingFee;` — these are the class's
  **fields**: the actual pieces of data every `Ticket` object will individually hold. `private`
  means only code *inside this Ticket class* can touch these directly — everyone else must go
  through the public getters/setters below.
- The **constructor** `public Ticket(String registrationNumber, double parkingFee)` — notice its
  name matches the class exactly (`Ticket`), and there is **no return type at all**, not even
  `void` — this is what makes it a constructor rather than a normal method. It runs the moment
  someone writes `new Ticket(...)`. Inside, `this.registrationNumber = registrationNumber;`
  takes the value that was just passed in as a parameter and stores it into **this particular
  object's own field**.
- `getRegistrationNumber()` — a getter: no parameters, returns the current value of the private
  field, letting outside code (like `App.java`) read it safely.
- `setRegistrationNumber(String registrationNumber)` — a setter: takes one parameter (the new
  value), and stores it into the field using `this.` to disambiguate from the parameter of the
  same name.
- `printTicket()` — an ordinary instance method (not a getter or setter), `void` because it just
  performs an action (printing) rather than handing back any value. Notice it's allowed to use
  `registrationNumber` and `parkingFee` directly (no `this.` strictly required here, since there's
  no parameter with the same name in this method to conflict with) — Java automatically knows
  these refer to the current object's own fields.

---

## Skill 4 — Array of `Ticket` objects + updated `parkCar()`

```java
private static Ticket[] tickets = new Ticket[100];
private static int ticketCount = 0;
```
- `private static Ticket[] tickets = new Ticket[100];` — a **global field** in `App.java`
  (`static`, matching everything else in this class). `Ticket[]` means "an array whose elements
  are `Ticket` objects" (not numbers this time — objects!). `new Ticket[100]` creates 100 empty
  "slots" — but critically, unlike `new int[100]` (which defaults every slot to `0`), each slot
  here defaults to **`null`** — meaning "no object here yet." You must explicitly create and
  place real `Ticket` objects into these slots yourself.
- `private static int ticketCount = 0;` — a simple counter tracking how many of those 100 slots
  actually contain a real object right now. Starts at 0 because no tickets exist yet.

```java
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
New pieces compared to Skill 2:
- `String reg = input.nextLine();` — reads a whole line of typed text (the registration number)
  and stores it in a `String` variable called `reg`. This happens **before** row/slot are asked
  for, exactly matching the requirement's specified order.
- `double fee = pricePerRow[row];` — this line only runs **inside the success branch** (after
  row is valid, slot is valid, AND the slot was actually available) — this is critical: the fee
  is only worked out once we already know the parking attempt is going to succeed.
  `pricePerRow[row]` looks up the price for that specific row from a separate array (e.g.
  `private static double[] pricePerRow = {4, 7, 7, 4};`, declared alongside `tickets`/`ticketCount`).
- `Ticket ticket = new Ticket(reg, fee);` — **creates a brand new `Ticket` object**, running its
  constructor with the registration number and fee just determined, and stores a reference to
  this new object in a local variable called `ticket`.
- `tickets[ticketCount] = ticket;` — places that new object into the next free slot of the
  global array — specifically, whichever slot number `ticketCount` currently points at (the
  first *empty* one, since we always increment after using a slot).
- `ticketCount++;` — increases the counter by 1, so the next time this runs, it uses the *next*
  slot along, and so the searching/saving methods later know exactly how many real tickets exist.

**Why must object creation/storing happen only inside the success branch?** If a Ticket were
created and stored even when the slot turned out to be occupied (or row/slot invalid), you'd end
up with a fake "receipt" for a parking action that never actually happened — the stored data
would misrepresent reality.

---

## Skill 5 — `searchTickets()`

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
- `double amount = input.nextDouble();` — reads a decimal number typed by the user (the target
  fee to search for).
- `boolean found = false;` — a **flag** variable, starting as "false" (meaning "nothing found
  yet"). We'll flip it to `true` the moment we find any match, and check it after the loop to
  decide whether to print the "not found" message.
- `for (int i = 0; i < ticketCount; i++)` — loops from `0` up to (but not including)
  `ticketCount`. **Crucially, it loops only up to `ticketCount`, not the array's full capacity
  of 100** — because slots from `ticketCount` onward are still empty (`null`), and trying to
  call a method on a `null` "object" (like `tickets[50].getParkingFee()` when only 10 tickets
  actually exist) throws a `NullPointerException` and crashes the program.
- `tickets[i].getParkingFee()` — `tickets[i]` gets the i-th `Ticket` object out of the array;
  `.getParkingFee()` then calls that specific object's getter method, returning its stored fee.
- `if (tickets[i].getParkingFee() == amount)` — compares that object's fee to what the user
  searched for. Since both are numbers (`double`), `==` is the correct comparison operator here
  (recall from Part 5's fields section: for comparing **Strings**, like registration numbers,
  you would use `.equals()` instead, e.g. `tickets[i].getRegistrationNumber().equals(searchValue)`).
- `System.out.println(tickets[i].getRegistrationNumber());` — if it matched, print that ticket's
  registration number.
- `found = true;` — remembers that at least one match has been found, so far.
- After the loop finishes entirely: `if (!found)` — `!` means NOT, so this reads "if found is NOT
  true" (i.e. it's still `false`, meaning the loop never found a single match) — print the "no
  payments were found" message.

---

## Skill 6 — `saveToFile()`

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
- `try { ... } catch (IOException e) { ... }` — because creating/writing to a file is a "risky"
  operation that Java forces you to handle (a **checked exception**), everything involving
  `FileWriter` must be wrapped like this. If anything goes wrong (e.g. permissions issue), the
  `catch` block runs instead of crashing the whole program.
- `FileWriter writer = new FileWriter("tickets.txt");` — creates a new file (or opens/overwrites
  an existing one) called `tickets.txt`, ready to accept text.
- `for (int i = 0; i < ticketCount; i++)` — same safe looping pattern as Skill 5 — only loop
  through the actual, real tickets that exist.
- `writer.write(tickets[i].getRegistrationNumber() + "," + tickets[i].getParkingFee());` — builds
  a single line of text by joining the registration number, a comma, and the fee together using
  `+` (String concatenation — when you use `+` with a String on one side, Java automatically
  converts the other side, e.g. the `double` fee, into text form too), then writes that whole
  line into the file.
- `writer.write(System.lineSeparator());` — writes a "start a new line" character sequence, so
  the *next* record (next loop iteration) begins on its own fresh line, rather than all records
  running together on one giant line.
- `writer.close();` — finishes and safely releases the file. Always do this once you're done
  writing.
- `System.out.println("Saved successfully.");` — a friendly confirmation message for the user,
  printed only if everything above succeeded without throwing an exception.

---

## You now understand every word of every skill area
Go back and re-read `2_Concept_Teaching_Full.md`, `3_CarParkManagement_Deep_Dive.md`,
`4_Practice_And_Test_Questions.md`, and `5_Full_Practice_Lab_Test.md` from your earlier study
pack — everything there will now be fully transparent, because there is no keyword or pattern
left unexplained. When you're ready, sit the full 90-minute practice test under timed conditions.
