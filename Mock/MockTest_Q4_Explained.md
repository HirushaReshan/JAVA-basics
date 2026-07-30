# Mock Test Question 4 — Fully Explained From Zero

This is usually the hardest question to *understand* (not necessarily to write), because it has
several parts that must happen in a very specific order. Let's slow right down.

## Decoding the question, sentence by sentence

> "Task: Modify the main method (App.java code given in link above) so it can handle objects of
> type Payment"

**Plain English:** Despite saying "main method," this really means "modify `App.java` overall" —
specifically, you'll add a new global variable and update `buyTicket()`, so the program can now
create and store `Payment` objects (using the class you built in Question 3).

> "1. In the main file (App.java), create a new array of objects of type Payment (as a global
> variable) that can store 100 payment objects."

**Plain English:** Add this line somewhere near the top of the `App` class (alongside
`planeSeats` and `pricePerRow`):
```java
private static Payment[] payments = new Payment[100];
```
"Global variable" means it's declared as a field of the class (not inside any one method), so
every method in `App.java` can see and use it. "Static" (implied, even though not explicitly
said) because everything in `App.java` is static, as covered in Zero Part 3/5 — `main()` needs
direct access without creating an `App` object.

You'll also need a **counter**, even though the question doesn't explicitly spell this out — you
need some way to track *how many* of the 100 slots are actually filled in, so later questions
(searching, saving) know where to stop looping:
```java
private static int paymentCount = 0;
```

> "2. Copy and paste the buyTicket() method you created in Question 2 and update the method to
> reflect requirements below."

**Plain English:** Start from your **Question 2 answer** (the version with row validation
already added) — don't start from scratch, and don't lose that validation. Then make further
changes on top of it.

> "a. Prompt user to enter the email address save it in a variable before user prompting row and
> seat numbers."

**Plain English:** Add a new line asking for the email, and it must go **first** — before the
existing row/seat prompts, not after.

> "b. Identify and store the payment amount, user need to pay based on the selected row and seat
> numbers. Payment amount should be decided based on the price plan listed below. This should be
> done only if the selected seats are available and purchase is successful."

**Plain English:** Work out how much this ticket costs, using the row the user picked (each row
has its own price, from the given `pricePerRow` array — you don't need to invent a new pricing
array, it already exists in the given code!). **Critically**: only do this calculation if the
seat turned out to be free and the purchase actually succeeds — not before you know that.

> "Each seat in row 1 costs £50. Each seat in row 2 costs £80. Each seat in row 3 costs £80. Each
> seat in row 4 costs £50."

**Plain English:** This is just describing what's already inside `pricePerRow` in the given code
(`pricePerRow[0] = 50; pricePerRow[1] = 80;` etc. — refer back to Question 1's explanation). You
don't need to write new numbers; you just need to **use** `pricePerRow[row]` to look up the right
price for whichever row was selected.

> "c. Create an object from Payment type to store user's email and payment amount and store the
> object inside the array you created in part 1 of this question. This should be done only if the
> seats are available and purchase is successful."

**Plain English:** Only if the purchase succeeded: build a new `Payment` object (using the
constructor from Question 3) with the email and the calculated price, then place it into the
`payments` array, and remember to update your counter.

> "Write your code in answer box according to below format:
> // 1.Code for creating Payment object Array
> //===============Add code here=======
> // 2.Code for updated buyTicket() method
> //===============Add code here========"

**Plain English:** They want your answer split into two clearly labelled parts: first the new
global array declaration(s), then the entire updated method.

---

## Step 1 — Recall your Question 2 answer as the starting point

```java
private static void buyTicket() {
    Scanner input = new Scanner(System.in);
    System.out.print("Enter row number: ");
    int row = input.nextInt() - 1;
    System.out.print("Enter seat number: ");
    int seat = input.nextInt() - 1;

    if (row < 0 || row > 3) {
        System.out.println("Invalid row number.");
    } else if (planeSeats[row][seat] == 0) {
        planeSeats[row][seat] = 1;
        System.out.println("Purchase successful.");
        showSeatingArea();
    } else {
        System.out.println("This seat is already taken.");
    }
}
```

## Step 2 — Adding the email prompt, in the correct position (BEFORE row/seat)

```java
Scanner input = new Scanner(System.in);

System.out.print("Enter email address: ");
String email = input.nextLine();

System.out.print("Enter row number: ");
int row = input.nextInt() - 1;
System.out.print("Enter seat number: ");
int seat = input.nextInt() - 1;
```
Notice `input.nextLine()` is used (not `nextInt()`), because an email is text, and it reads the
**whole line** typed by the user, not just one number.

⚠️ **A well-known Scanner gotcha:** if this method is called right after another method used
`input.nextInt()` to read the user's menu choice (in `getOption()`), there may be a leftover
"Enter" keypress still sitting in the input buffer, which can make the very next `nextLine()`
call appear to be skipped. If your testing shows the email prompt seems to get bypassed, this is
the well-known cause — you'd fix it with an extra `input.nextLine();` immediately before reading
the email, to clear that leftover character. Keep this in mind if you're testing this in
IntelliJ, though the exact provided project's exam version may already avoid this by how `main`
and menu handling are structured.

## Step 3 — Where does the price calculation and object-creation go?

This is the part students get wrong most often: **it must go inside the success branch only** —
meaning, only in the part of the `if/else if/else` chain where the seat was confirmed available.

```java
} else if (planeSeats[row][seat] == 0) {
    planeSeats[row][seat] = 1;

    int amount = pricePerRow[row];              // NEW — calculate the price
    Payment payment = new Payment(email, amount); // NEW — create the object
    payments[paymentCount] = payment;              // NEW — store it in the array
    paymentCount++;                                 // NEW — update the counter

    System.out.println("Purchase successful. Amount: £" + amount);
    showSeatingArea();
} else {
    System.out.println("This seat is already taken.");
}
```

Let's break down each new line:
- `int amount = pricePerRow[row];` — looks up the price for the exact row that was purchased.
  Since `pricePerRow` already exists in the given code (from Question 1's `initialiseRows()`
  method) and is indexed the same way as `planeSeats` (row 0 = first row, etc.), this single line
  correctly gets £50 or £80 depending on which row was picked. This is only calculated **here**,
  inside the success branch — never before we know the purchase succeeded.
- `Payment payment = new Payment(email, amount);` — creates a brand-new `Payment` object,
  running the constructor from Question 3, passing in the email that was collected earlier and
  the price just calculated.
- `payments[paymentCount] = payment;` — stores this new object into the next free slot of the
  global array.
- `paymentCount++;` — increases the counter, so the array knows one more real object now exists.

**Why must this all happen only inside the success branch?** If the seat turned out to be
already taken, no purchase actually happened — so it would be wrong to still create a `Payment`
record charging the user for something that never went through.

---

## Step 4 — The complete final answer, in the requested format

```java
// 1.Code for creating Payment object Array
private static Payment[] payments = new Payment[100];
private static int paymentCount = 0;

// 2.Code for updated buyTicket() method
private static void buyTicket() {
    Scanner input = new Scanner(System.in);

    System.out.print("Enter email address: ");
    String email = input.nextLine();

    System.out.print("Enter row number: ");
    int row = input.nextInt() - 1;
    System.out.print("Enter seat number: ");
    int seat = input.nextInt() - 1;

    if (row < 0 || row > 3) {
        System.out.println("Invalid row number.");
    } else if (planeSeats[row][seat] == 0) {
        planeSeats[row][seat] = 1;

        int amount = pricePerRow[row];
        Payment payment = new Payment(email, amount);
        payments[paymentCount] = payment;
        paymentCount++;

        System.out.println("Purchase successful. Amount: £" + amount);
        showSeatingArea();
    } else {
        System.out.println("This seat is already taken.");
    }
}
```

---

## Why this matters for the real exam

The real Question 4 will ask for essentially the same structure applied to Car Park (a new field
like a vehicle registration number, prompted first; a price/fee calculated per row; a new
`Ticket`/`Vehicle` object created and stored only on success). The **exact skill** — correct
order of operations (collect input → validate → check availability → calculate → create → store)
— is what's really being tested, not the specific field names.

## Quick self-check
1. Why must the email be read with `nextLine()` instead of `nextInt()`?
2. Why must the email prompt come before the row/seat prompts in the code, matching the question?
3. Why is the price calculation placed inside the `else if (planeSeats[row][seat] == 0)` branch,
   and not right at the top of the method?
4. What would go wrong if you forgot to write `paymentCount++;` after storing a new Payment
   object?
5. Why don't we need to create a brand-new pricing array for this question?

*(1: because an email is text (a String), not a whole number, so it must be read differently
using nextLine(). 2: because the question explicitly specifies that order — "before user
prompting row and seat numbers." 3: because the calculation should only happen once we know the
purchase is actually going through (the seat was free) — doing it earlier would calculate a price
for a purchase that might not even happen. 4: the next payment created would overwrite the same
array slot, and earlier or later payments could be lost or missed when searching/saving. 5: because
`pricePerRow` already exists in the given code from `initialiseRows()`, indexed the same way as
`planeSeats` — you just reuse it with `pricePerRow[row]`.)*
