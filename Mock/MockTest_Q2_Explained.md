# Mock Test Question 2 Fully Explained From Zero

## Decoding the question, sentence by sentence

> "Task: Validating seat Row numbers"

**Plain English:** "Validating" means "checking that something is correct/acceptable before
using it." So this task is about **checking the row number the user types in**, before letting
them actually buy a seat there.

> "Refer to buyTicket() method in line number 75-91 of sample code."

**Plain English:** Go look at lines 75 to 91 in the given code file that's where the method
called `buyTicket()` lives. This tells you exactly which method you're allowed/expected to
change.

> "add the correct code to perform the validation for the row number as follows:
> Row number cannot be smaller than 1 (rows start at 1)
> Row number cannot be larger than the number of rows in the plane (which is 4)."

**Plain English:** You need to add `if` checks (from your Zero-knowledge Part 2) that reject:
- Any row number **below** 1 (e.g. 0, -1, -5 typed by the user)
- Any row number **above** 4 (e.g. 5, 100 typed by the user)

If the row number fails either of these checks, the program should say so, instead of trying to
use an invalid row and crashing.

> "Write the updated buyTicket() method in the answer box below."

**Plain English:** Just like Question 1, you give back the **whole method**, not just the new
lines but this time, the method you're rewriting is `buyTicket()`, and you're adding validation
logic to it.

---

## Step 1 The original, unmodified method (given to you)

```java
75 private static void buyTicket() {
76     Scanner input = new Scanner(System.in);
77     System.out.print("Enter row number: ");
78     int row = input.nextInt() - 1;
79     System.out.print("Enter seat number: ");
80     int seat = input.nextInt() - 1;
81
82     // Check if the seat is available or not
83     if (planeSeats[row][seat] == 0) {
84         planeSeats[row][seat] = 1;
85         System.out.println("Purchase successful.");
86         showSeatingArea();
87     } else {
88         System.out.println("This seat is already taken.");
89     }
90 }
```

## Step 2 Understanding every line, so you know exactly what's happening before you add anything

- `Scanner input = new Scanner(System.in);` creates a tool to read what the user types on the
  keyboard.
- `System.out.print("Enter row number: ");` displays a prompt asking for the row.
- `int row = input.nextInt() - 1;` reads the number the user typed, then **immediately
  subtracts 1**. This is critical to understand: if the user types `1` (meaning "row 1" in human
  terms), the variable `row` actually ends up storing `0` (the correct zero-indexed array
  position for the first row). If the user types `4`, `row` ends up storing `3`.
- The same thing happens for `seat`.
- `if (planeSeats[row][seat] == 0)` this is the **only** check currently in the method: "is
  this specific seat available?" **There is currently no check at all on whether `row` or `seat`
  are sensible numbers.** If the user types `9` for the row, `row` becomes `8`, and
  `planeSeats[8][...]` doesn't exist (only indexes 0–3 exist, since there are 4 rows) this
  would crash the program immediately with an error.

**This is exactly the gap the question wants you to fix.**

---

## Step 3 Figuring out the correct boundary numbers (the trickiest part of this question)

The question says, in **human/1-based terms**:
- Row cannot be smaller than 1
- Row cannot be larger than 4

But remember: by the time you're checking `row` inside the method, it has **already had `- 1`
subtracted from it** (line 78). So you must translate these human-facing rules into **zero-indexed
terms**, matching what `row` actually contains at that point in the code:

| Human-facing rule | What the user typed | What `row` actually holds (after `-1`) |
|---|---|---|
| "cannot be smaller than 1" | user types `1` (smallest valid) | `row` = `0` (smallest valid) |
| | user types `0` (invalid, too small) | `row` = `-1` (invalid) |
| "cannot be larger than 4" | user types `4` (largest valid) | `row` = `3` (largest valid) |
| | user types `5` (invalid, too big) | `row` = `4` (invalid) |

So, in terms of the actual `row` variable, valid values are `0, 1, 2, 3`. Anything **less than
0**, or **greater than 3**, is invalid. This gives us the condition:

```java
if (row < 0 || row > 3) {
    System.out.println("Invalid row number.");
}
```

**Why `> 3` and not `> 4`?** Because `row` has already been reduced by 1. The 4th (last) row, when
typed as `4` by the user, becomes `3` internally. So "larger than the last valid row" means
`row > 3`, not `row > 4`. This is the single most common mistake students make on this exact
question mixing up whether you're validating the "before `-1`" number or the "after `-1`"
number. **Always validate the actual variable as it exists at that point in the code.**

---

## Step 4 Fitting this into the existing if/else structure correctly

You now have **two decisions** to make in sequence:
1. Is the row number valid at all?
2. *(Only if yes)* Is the seat actually available?

This is a perfect use of `if / else if / else` (from Zero Part 2) chained together so only one
message ever prints:

```java
if (row < 0 || row > 3) {
    System.out.println("Invalid row number.");
} else if (planeSeats[row][seat] == 0) {
    planeSeats[row][seat] = 1;
    System.out.println("Purchase successful.");
    showSeatingArea();
} else {
    System.out.println("This seat is already taken.");
}
```

Read this out loud as a decision tree: *"First, check if the row is invalid if so, say so and
stop. Otherwise (meaning the row WAS valid), check if the seat is free if so, buy it and show
the seating area. Otherwise (row was valid, but seat wasn't free), say it's already taken."*

---

## Step 5 The complete final answer

```java
private static void buyTicket() {
    Scanner input = new Scanner(System.in);
    System.out.print("Enter row number: ");
    int row = input.nextInt() - 1;
    System.out.print("Enter seat number: ");
    int seat = input.nextInt() - 1;

    // Check if the seat is available or not
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

Notice everything above line 82 stays exactly as given you didn't need to touch how input is
read, only add validation **before** the existing seat-availability check is allowed to run.

---

## Why this matters for the real exam

The real exam's Question 2 will likely ask you to validate the **parking row** (and possibly
also the **parking slot**) in `parkCar()`, using the exact same logic pattern:
```java
if (row < 0 || row > 3) {
    System.out.println("Invalid row number.");
} else if (slot < 0 || slot >= parkingSlots[row].length) {
    System.out.println("Invalid slot number.");
} else if (parkingSlots[row][slot] == 0) {
    ...
}
```
The skill being tested is **identical**: translate a human-facing range rule into the correct
zero-indexed boundary check, and insert it as the first condition in an `if/else if/else` chain,
*before* any code that actually uses the row/slot to access the array.

## Quick self-check
1. Why is the condition `row < 0 || row > 3` and not `row < 1 || row > 4`?
2. What would happen if you put the row-validation check *after* the
   `planeSeats[row][seat] == 0` check instead of before it?
3. Why does `||` (OR) get used here instead of `&&` (AND)?
4. If the exam also asked you to validate the seat number (not just the row), what similar
   condition would you add, and where?

*(1: because `row` has already had `-1` subtracted from the user's typed value by the time it's
checked, so the valid range 1–4 (human) becomes 0–3 (code). 2: if the row is invalid e.g. `row`
= 8 Java would try to access `planeSeats[8][seat]`, which doesn't exist, and crash with an
`ArrayIndexOutOfBoundsException`, before your validation message ever gets a chance to print. 3:
because a single number can never be BOTH less than 0 AND greater than 3 at the same time using
`&&` here would make the condition impossible to ever be true. 4: something like
`else if (seat < 0 || seat >= planeSeats[row].length)`, placed as an additional `else if` between
the row check and the availability check.)*
