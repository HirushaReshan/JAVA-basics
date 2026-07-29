# Java From Absolute Zero  Part 2: Control Flow

Control flow means: **deciding which code runs, and how many times it runs.** Without this,
your program could only ever run every line, once, top to bottom, with no decisions or repeats.

---

## 1. `if / else if / else`  making decisions

```java
int row = 5;

if (row < 0) {
    System.out.println("Row too small.");
} else if (row > 3) {
    System.out.println("Row too big.");
} else {
    System.out.println("Row is valid.");
}
```

Word by word:
- `if`  a keyword meaning "check whether this condition is true; if so, run the code in the
  braces that follow."
- `(row < 0)`  the **condition**. Must be something that evaluates to `true` or `false`.
- `{ System.out.println(...); }`  the **body**. Only runs if the condition directly above it was
  true.
- `else if (row > 3)`  "if the FIRST condition was false, check THIS condition instead."
- `else`  "if NONE of the above conditions were true, run this instead." No condition needed 
  it's the catch-all.

**Important:** Java checks these top to bottom, and stops at the **first** one that's true. It
will never run more than one branch of the same if/else-if/else chain.

```java
int row = 5;
if (row < 0) {
    System.out.println("A");
}
if (row > 3) {          // ⚠ this is a SEPARATE if, not "else if"  Java checks it independently
    System.out.println("B");
}
```
Here, if `row` is 5, only `"B"` prints  because these are two separate `if` statements, not
one chain. This distinction matters a lot in the exam: chaining with `else if` guarantees only
one message ever prints; separate `if` statements can allow multiple messages to print.

### Nesting `if` statements
```java
if (row >= 0 && row <= 3) {
    if (slot >= 0 && slot < parkingSlots[row].length) {
        System.out.println("Both valid.");
    }
}
```
You can put an `if` **inside** another `if`. This is called "nesting," and is often equivalent to
combining both conditions with `&&` in one `if`  use whichever is clearer for the specific task.

---

## 2. `switch` statements  choosing between many exact values

```java
int option = 2;

switch (option) {
    case 0:
        System.out.println("You chose zero.");
        break;
    case 1:
        System.out.println("You chose one.");
        break;
    case 2:
        System.out.println("You chose two.");
        break;
    default:
        System.out.println("Not a valid option.");
}
```

- `switch (option)`  "look at the value of `option`, and jump to the matching `case` below."
- `case 0:`  "if `option` equals exactly `0`, start running code from here."
- `break;`  **stops** the switch here, jumping to the code after the whole switch block. Without
  `break`, Java would keep running the code in the *next* case too ("fall-through")  usually
  not what you want, and a common bug if forgotten.
- `default:`  runs if `option` didn't match **any** of the `case` values. Like `else` for
  switches. Doesn't strictly need a `break` since it's last, but it's good practice.

**When to use `switch` vs `if/else`:** `switch` is used when you're comparing **one variable**
against several **exact, specific values** (like a menu option 0, 1, 2). `if/else` is more
flexible and used for ranges/comparisons (`row < 0`, `age >= 18`, etc.).

---

## 3. Loops  repeating code

### `for` loop  when you know how many times to repeat (or can count)
```java
for (int i = 0; i < 5; i++) {
    System.out.println("i is now " + i);
}
```
A `for` loop has **three parts**, separated by semicolons:
1. `int i = 0;`  **initialisation**. Runs once, right at the start. Creates a counter variable.
2. `i < 5;`  **condition**. Checked *before* every loop run. If true, the loop body runs. If
   false, the loop stops.
3. `i++`  **update**. Runs *after* every loop body completes, before checking the condition
   again. `i++` means "increase `i` by 1" (shorthand for `i = i + 1`).

Trace through it: `i=0` (0<5 ✓, prints "0", then i becomes 1) → `i=1` (1<5 ✓, prints "1", i
becomes 2) → ... → `i=5` (5<5 ✗, loop stops). So it prints `0,1,2,3,4`  **five** times total,
never reaching 5 itself. This is why array loops almost always use `i < array.length`, not
`i <= array.length`  array indexes go from `0` to `length - 1`.

### `while` loop  repeats *while* a condition stays true (condition checked first)
```java
int count = 0;
while (count < 3) {
    System.out.println("Count is " + count);
    count++;
}
```
Same idea as `for`, but you manage the counter yourself, and it's used when you don't necessarily
know in advance how many times you'll loop (e.g. "keep asking until the user enters valid input").

### `do-while` loop  like `while`, but checks the condition **after** running the body once
```java
int option;
do {
    System.out.print("Enter an option: ");
    option = input.nextInt();
} while (option < 0 || option > 2);
```
This guarantees the body runs **at least once**, even if the condition would have been false
immediately  perfect for "ask the user for input, then keep re-asking until it's valid,"
because you always need to ask at least once.

### Comparing all three
| Loop | Checks condition | Guaranteed at least 1 run? | Best for |
|---|---|---|---|
| `for` | before each run | No | Known number of repetitions (e.g. looping through an array) |
| `while` | before each run | No | Unknown number of repetitions, condition might be false from the start |
| `do-while` | after each run | **Yes** | "Ask once, then repeat until valid" input patterns |

---

## 4. `break` and `continue` inside loops

```java
for (int i = 0; i < 10; i++) {
    if (i == 5) {
        break; // immediately exits the loop entirely  nothing after runs, even for later i values
    }
    System.out.println(i);
}
// prints 0,1,2,3,4, then stops completely
```

```java
for (int i = 0; i < 5; i++) {
    if (i == 2) {
        continue; // skips the REST of this one loop cycle, then moves on to the next i
    }
    System.out.println(i);
}
// prints 0,1,3,4  "2" is skipped, but the loop still continues afterward
```

---

## 5. Nested loops (a loop inside a loop)

This is exactly what `showParkingLayout()` uses:
```java
for (int row = 0; row < 4; row++) {        // outer loop: goes through each row
    for (int slot = 0; slot < 20; slot++) { // inner loop: goes through each slot IN that row
        System.out.print("[O]");
    }
    System.out.println(); // after finishing all slots in one row, move to a new line
}
```
For every single run of the **outer** loop (each row), the **entire inner** loop runs completely
(all its slots) before the outer loop moves to its next row. This is exactly how you process
anything 2D  a grid, a 2D array, a table.

---

## Quick Self-Test
1. What's the difference between `if/else if/else` and multiple separate `if` statements?
2. Why does `for (int i = 0; i < arr.length; i++)` never try to access an invalid index?
3. What does forgetting `break` in a `switch` case cause?
4. When would you choose `do-while` over a plain `while`?
5. In a nested loop, which loop finishes completely first for each cycle of the other  inner or outer?

**Next: [Part 3  Methods (what they are, and every keyword involved)](Zero_Part3_Methods.md)**
