# Java From Absolute Zero  Part 4: Arrays

## 1. Why do we need arrays?

Imagine you need to store the ages of 100 students. Without arrays, you'd need 100 separate
variables (`age1, age2, age3, ... age100`)  completely unmanageable. An **array** is a single
variable that holds **multiple values of the same type**, all grouped together, accessible by
position (**index**).

---

## 2. Declaring and using a 1D (one-dimensional) array

```java
int[] scores = new int[5];
```
Word by word:
- `int[]`  the **type** of the array: "an array of `int` values." The square brackets `[]` are
  what make it an array type instead of a single value.
- `scores`  the **variable name**.
- `new`  a keyword meaning "create a brand new thing in memory." (We'll see `new` again heavily
  with objects in Part 5  it always means "actually build/allocate this now.")
- `int[5]`  "make room for exactly 5 `int` values."

This creates 5 "slots," each initially holding the default value for `int`, which is `0`.
(Default values: `int`→`0`, `double`→`0.0`, `boolean`→`false`, objects/`String`→`null`.)

### Indexing  accessing individual slots
```java
scores[0] = 90;  // put 90 into the FIRST slot
scores[1] = 85;  // put 85 into the SECOND slot
System.out.println(scores[0]); // prints 90
```
⚠️ **Arrays are zero-indexed.** The first item is at index `0`, not `1`. For an array of 5
items, valid indexes are `0, 1, 2, 3, 4`  index `5` does **not exist** and would crash your
program.

### Array literal shorthand (creating with values immediately known)
```java
int[] scores = {90, 85, 77, 100, 60};
```

### The `.length` property
```java
System.out.println(scores.length); // 5
```
⚠️ `.length` is a **property**, not a method  no parentheses `()` after it. (Compare: for
`String`, it's `.length()` **with** parentheses  a common source of confusion, since Strings
work slightly differently under the hood. For arrays: no brackets.)

### Looping through an array
```java
for (int i = 0; i < scores.length; i++) {
    System.out.println(scores[i]);
}
```
Using `scores.length` instead of a hardcoded number (like `5`) means your loop **automatically
still works correctly** even if the array's size changes later.

---

## 3. What happens if you access an invalid index?

```java
int[] scores = new int[5];
System.out.println(scores[5]); // ❌ CRASH
```
This throws an `ArrayIndexOutOfBoundsException`  the program stops abruptly (unless you catch
the exception  see Part 6). This is **exactly** the bug that input validation (from Part 2 and
your exam Question 2 style tasks) is designed to prevent  always check `index >= 0` and
`index < array.length` before touching an array position.

---

## 4. 2D Arrays (rows and columns  a grid)

```java
int[][] grid = new int[3][4]; // 3 rows, each with EXACTLY 4 columns
```
This is a **rectangular** 2D array  every row has the same length. Access with two indexes:
```java
grid[0][0] = 1; // row 0, column 0
grid[2][3] = 9; // row 2, column 3 (the last valid cell)
```

---

## 5. Jagged Arrays (rows of *different* lengths)  used in the Car Park project

Sometimes you need each row to have a **different** number of columns. Java allows this because
technically, a 2D array in Java is really just "an array, where each element is itself another
array"  and those inner arrays don't have to be the same length.

```java
int[][] parkingSlots = new int[4][];
```
Reading this: "create an array of 4 slots, where each slot will itself hold **another array of
ints**  but don't decide each inner array's length yet (that's what the empty `[]` means)."

At this point, `parkingSlots[0]`, `parkingSlots[1]`, etc. are all still `null` (empty/undefined)
 you haven't told Java how long each row is yet.

```java
parkingSlots[0] = new int[18]; // NOW row 0 is a real array of 18 ints, all defaulting to 0
parkingSlots[1] = new int[20]; // row 1 has 20 ints
parkingSlots[2] = new int[20]; // row 2 has 20 ints
parkingSlots[3] = new int[18]; // row 3 has 18 ints
```
Each of these lines **separately creates a brand new int array** and assigns it into that
position of the outer array. This is why, when you're asked to "change the number of slots per
row," you only ever touch the number inside these brackets  the structure (`new int[4][]`, then
4 separate row-creation lines) doesn't change.

### Accessing and looping through a jagged array
```java
parkingSlots[1][5] = 1; // row 1, slot 5 (both zero-indexed)

for (int row = 0; row < parkingSlots.length; row++) {          // loop over ALL rows
    for (int slot = 0; slot < parkingSlots[row].length; slot++) { // loop over THIS row's slots
        System.out.print(parkingSlots[row][slot]);
    }
    System.out.println();
}
```
Notice: `parkingSlots[row].length`  this gives the length of **that specific row's array**,
which is why the inner loop correctly stops at 18 for rows 0/3 but at 20 for rows 1/2, even
though they're different.

---

## 6. Arrays of Objects (a quick preview  fully explained once you know OOP in Part 5)

Arrays don't just hold numbers  they can hold **objects** too:
```java
Ticket[] tickets = new Ticket[100];
```
This creates 100 "slots" that can each eventually hold a `Ticket` object  but right now, every
slot is `null` (empty) until you actually create and store real `Ticket` objects inside them.
This is exactly how your exam's "array of 100 Payment/Ticket/Vehicle objects" requirement works 
covered in depth in Part 5.

---

## Quick Self-Test
1. What value does an uninitialised `int` array slot hold by default?
2. Why does `int[4][20]` NOT work for rows that need different lengths?
3. What Java exception happens if you access index 5 in a 5-element array?
4. In `parkingSlots[row].length`, what exactly does this expression measure?
5. What does `new int[18]` actually do?

**Next: [Part 5  Object-Oriented Programming (classes, objects, constructors, getters/setters  the big one)](Zero_Part5_OOP.md)**
