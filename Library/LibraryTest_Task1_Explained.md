# Library System — Task 1 Explained (Add a Third Row of Shelves)

## ⚠️ A note before we start
You haven't uploaded the actual `App.java` for this Library project, only the task wording. Based
on the line numbers mentioned across all 6 tasks (`runMenu()` at 25-47, `getOption()` at 49-60,
`returnBook()` at 92-109) and the visible snippet in your screenshot
(`public static void initialiseShelves(){`), this project follows the **exact same structure**
as your Plane App and Car Park App — just themed around a library. I've reconstructed the likely
original code below so the teaching makes sense; if your real file differs slightly in variable
names, the *logic and method you write* will still be correct — just match your variable names
exactly on the day.

**Assumed original code (before your changes):**
```java
private static int[][] bookShelves = null;

public static void initialiseShelves() {
    bookShelves = new int[2][];
    bookShelves[0] = new int[10]; // Shelf 1
    bookShelves[1] = new int[15]; // Shelf 2
}
```
(`0` = book available on that shelf position, `1` = book currently borrowed/unavailable — same
convention as "available/occupied" in your other two projects.)

---

## Decoding the question, sentence by sentence

> "Refer to the App.java file and identify the method that needs to be updated..."

**Plain English:** Exactly like Question 1 in your Plane App mock — find the ONE method
responsible for setting up the shelves/books, and change only that method.

> "Add a third row of shelves (Shelf 3)."

**Plain English:** Right now there are only 2 shelves (indexes `0` and `1`). You need to add a
**third** one (index `2`).

> "Shelf 3 should contain 10 books and ensure all books are available at the start of the
> program."

**Plain English:** Shelf 3's array should have 10 slots. "Ensure all books are available at the
start" — this is a **hint, not extra work**: in Java, a `new int[10]` array is **automatically**
filled with `0` in every slot by default (recall Zero-knowledge Part 4). Since `0` already means
"available" in this project's convention, you don't need to write any extra code to make them
available — creating the array with `new int[10]` already achieves this. This sentence is testing
whether you *understand* that default array values already satisfy the requirement, not asking
you to loop through and manually set each slot to available.

> "Update and write the identified method below to reflect the requirements above (Note that you
> need to write the complete method with updated code, in the answer box below)."

**Plain English:** Same instruction style as always — give back the **entire method**, not just
the new line.

---

## Step 1 — What has to change structurally

Adding a third row means the **outer array size** itself must grow, not just adding a new
assignment line to a still-2-sized array:
```java
bookShelves = new int[2][];   // OLD — only room for 2 rows
```
must become:
```java
bookShelves = new int[3][];   // NEW — room for 3 rows
```
If you forget to change the `2` to `3` here, but still try to write
`bookShelves[2] = new int[10];` below, your program will crash with an
`ArrayIndexOutOfBoundsException`, because the outer array was never actually given a 3rd slot to
put anything into.

## Step 2 — Adding the new row's assignment line

```java
bookShelves[2] = new int[10]; // Shelf 3
```
Index `2` is correct because Shelf 1 = index `0`, Shelf 2 = index `1`, so Shelf 3 = index `2`
(zero-indexing, as always).

---

## Step 3 — The complete final answer

```java
public static void initialiseShelves() {
    bookShelves = new int[3][];
    bookShelves[0] = new int[10]; // Shelf 1
    bookShelves[1] = new int[15]; // Shelf 2
    bookShelves[2] = new int[10]; // Shelf 3
}
```

Notice: Shelf 1 and Shelf 2's lines are **completely unchanged** from the original — you're not
asked to modify them, only to add a third one, and remember to bump the outer array size from 2
to 3.

---

## Quick self-check
1. Why must `new int[2][]` become `new int[3][]`, rather than just adding a new line
   `bookShelves[2] = new int[10];` on its own?
2. Why doesn't "ensure all books are available at the start" require any extra code?
3. What Java exception would you get if you added the third row's line but forgot to resize the
   outer array?
4. Why is the new shelf placed at index `2`, not index `3`?

*(1: because the outer array's size determines how many row "slots" exist at all — if it's still
sized for 2, there is no index 2 to assign into, no matter what you write on a new line. 2:
because `new int[10]` already defaults every slot to `0`, and `0` already means "available" in
this project's convention — the default behaviour already satisfies the requirement. 3:
`ArrayIndexOutOfBoundsException`, because index 2 wouldn't exist in an array only sized for 2
elements (valid indexes 0 and 1 only). 4: because array indexing is zero-based, and this is the
third shelf overall — the first two shelves already occupy indexes 0 and 1.)*
