# Mock Test Question 1 — Fully Explained From Zero

## First, let's decode what the question is actually asking, sentence by sentence

> "In this task, you are requested to change the number of seats per row in the
> 4COSC010C_Mock_PlaneApp project provided."

**Plain English:** The project has a plane with rows of seats. Right now, each row has a certain
number of seats already set up in the code. Your job is to make each row have a **different**
number of seats than it currently does.

> "Refer to the sample code link provided above, and identify the method you need to modify."

**Plain English:** Somewhere in the given code, there's ONE specific method responsible for
setting up how many seats each row has. You need to find it. (A "method" — remember from your
zero-knowledge lessons — is a named, reusable block of code that does one job.)

> "Rewrite the complete method to change the number of seats per row as follows:
> Row 1 should have 16 seats. Row 2 should have 22 seats. Row 3 should have 22 seats.
> Row 4 should have 16 seats."

**Plain English:** Once you've found that method, don't just describe the change — actually
**type out the entire method again**, from its very first line to its very last closing brace,
but with the seat numbers changed to 16, 22, 22, 16.

> "(You ONLY need to rewrite the COMPLETE method related to satisfying above requirement)"

**Plain English:** Don't rewrite the whole `App.java` file. Don't rewrite `main()`. Just find the
**one** method that sets up seat counts, and rewrite **only that one method**, completely.

---

## Step 1 — Finding the right method in the given code

Looking at the provided sample code (`App.java`), here is the method that sets up the plane's
seating, exactly as given:

```java
14 public static void initialiseRows() {
15     planeSeats = new int[4][];
16     planeSeats[0] = new int[18]; // row 1 - initialised
17     planeSeats[1] = new int[20]; // row 2 - initialised
18     planeSeats[2] = new int[20]; // row 3 - initialised
19     planeSeats[3] = new int[18]; // row 4 - initialised
20     pricePerRow = new int[4];
21     pricePerRow[0] = 50;
22     pricePerRow[1] = 80;
23     pricePerRow[2] = 80;
24     pricePerRow[3] = 50;
25 }
```

**How do we know this is the right method?** The method's name is literally
`initialiseRows()` — "initialise" means "set up for the first time." Inside it, we can see
`planeSeats[0] = new int[18];` — this is creating an array of 18 seats and storing it as "row 1"
(remember: index `0` in the code = "row 1" to the human user, because arrays are zero-indexed —
covered in your Zero-knowledge Part 4). The comment `// row 1 - initialised` next to it confirms
this. So this method is exactly where the seat counts per row are decided.

---

## Step 2 — Understanding exactly what each line does (so you know what NOT to touch)

| Line | What it does |
|---|---|
| `planeSeats = new int[4][];` | Creates a jagged 2D array with 4 rows, but doesn't yet decide how many seats are in each row. |
| `planeSeats[0] = new int[18];` | Row 1 (index 0) gets its own array of 18 seats. |
| `planeSeats[1] = new int[20];` | Row 2 (index 1) gets its own array of 20 seats. |
| `planeSeats[2] = new int[20];` | Row 3 (index 2) gets its own array of 20 seats. |
| `planeSeats[3] = new int[18];` | Row 4 (index 3) gets its own array of 18 seats. |
| `pricePerRow = new int[4];` | Creates a separate array to hold the ticket price for each row. |
| `pricePerRow[0] = 50;` etc. | Sets the price for each row (£50 for rows 1/4, £80 for rows 2/3). |

**Important realisation:** the question ONLY asks you to change the **number of seats**, not the
**prices**. So the `pricePerRow` lines stay **exactly the same** — you only touch the numbers
inside the four `planeSeats[...] = new int[...]` lines.

---

## Step 3 — Making the actual change

The question wants:
- Row 1 → 16 seats
- Row 2 → 22 seats
- Row 3 → 22 seats
- Row 4 → 16 seats

Row 1 is `planeSeats[0]`, Row 2 is `planeSeats[1]`, Row 3 is `planeSeats[2]`, Row 4 is
`planeSeats[3]` — remember, "Row 1" (what the human sees) always maps to index `0` (what the
code uses), because of zero-indexing.

So:
- `planeSeats[0] = new int[18];` becomes `planeSeats[0] = new int[16];`
- `planeSeats[1] = new int[20];` becomes `planeSeats[1] = new int[22];`
- `planeSeats[2] = new int[20];` becomes `planeSeats[2] = new int[22];`
- `planeSeats[3] = new int[18];` becomes `planeSeats[3] = new int[16];`

---

## Step 4 — The complete final answer

```java
public static void initialiseRows() {
    planeSeats = new int[4][];
    planeSeats[0] = new int[16]; // row 1 - initialised
    planeSeats[1] = new int[22]; // row 2 - initialised
    planeSeats[2] = new int[22]; // row 3 - initialised
    planeSeats[3] = new int[16]; // row 4 - initialised
    pricePerRow = new int[4];
    pricePerRow[0] = 50;
    pricePerRow[1] = 80;
    pricePerRow[2] = 80;
    pricePerRow[3] = 50;
}
```

Notice: **everything else in the method stayed identical** — the method name, the braces, the
`pricePerRow` section — because the question only asked about seat counts. This is what
"rewrite the complete method" means: give the whole thing back, unchanged except for the one
specific detail asked about.

---

## Why this matters for the real exam

The real exam's Question 1 will ask the exact same *type* of thing, but about the Car Park
project instead — probably asking you to change the number of **parking slots per row** in
`initialiseParkingSlots()`. The method will look almost identical in shape:
```java
public static void initialiseParkingSlots() {
    parkingSlots = new int[4][];
    parkingSlots[0] = new int[18]; // Row A
    parkingSlots[1] = new int[20]; // Row B
    parkingSlots[2] = new int[20]; // Row C
    parkingSlots[3] = new int[18]; // Row D
}
```
The car park version has no `pricePerRow` section built in yet (you might add one yourself in a
later question), but the core skill — find the method that creates the jagged array, and only
change the numbers inside the brackets — is identical.

## Quick self-check
1. Why does "Row 1" correspond to `planeSeats[0]` and not `planeSeats[1]`?
2. Why did the `pricePerRow` lines not need to change in this question?
3. What would happen if you wrote `planeSeats[4] = new int[16];` by mistake?
4. Why is it important to rewrite the *entire* method rather than just the changed lines, when
   the question specifically asks for the "complete method"?

*(1: because arrays are zero-indexed, so the first row is always at index 0. 2: because the
question only asked about seat counts, not prices — nothing about pricing was mentioned. 3: it
would crash with an `ArrayIndexOutOfBoundsException`, because the array only has valid indexes
0–3 (created as `new int[4][]`) — index 4 doesn't exist. 4: because "complete method" is an
explicit instruction — partial answers likely lose marks even if the core logic is correct,
since the examiner wants to see the whole method compiles and works as a unit.)*
