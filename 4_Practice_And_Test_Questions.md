# Practice & Concept-Testing Questions

Use these to test your understanding of each *isolated* concept before attempting the full
timed practice test (file 5). Try to answer without looking back at your notes first — that's
the real test of whether you've mastered it.

Answers are in **`6_Answers_And_Cheat_Sheet.md`**.

---

## Section 1 — Arrays

**Q1.1.** What is wrong with this code, if the goal is to have Row A hold 10 slots and Row B hold
15 slots?
```java
int[][] parkingSlots = new int[2][15];
```

**Q1.2.** Write a jagged array declaration (from scratch) for 3 rows, holding 5, 8, and 12 slots
respectively.

**Q1.3.** Given `parkingSlots[2].length`, what does this return?

**Q1.4.** Trace through this loop and write down what gets printed:
```java
int[] arr = {3, 6, 9};
for (int i = 0; i < arr.length; i++) {
    System.out.print(arr[i] + " ");
}
```

---

## Section 2 — Validation

**Q2.1.** A method does `int row = input.nextInt() - 1;`. The requirement says "row cannot be
smaller than 1, cannot be larger than 5." Write the correct `if` condition to catch invalid input
(using the `row` variable, which is already adjusted by `-1`).

**Q2.2.** Rewrite this so that an occupied-slot message is only shown when the row/slot are
actually valid, otherwise show an "invalid" message instead — don't let it try to access the
array with a bad index:
```java
private static void parkCar() {
    Scanner input = new Scanner(System.in);
    int row = input.nextInt() - 1;
    int slot = input.nextInt() - 1;
    if (parkingSlots[row][slot] == 0) {
        parkingSlots[row][slot] = 1;
        System.out.println("Vehicle parked successfully.");
    } else {
        System.out.println("This parking slot is already occupied.");
    }
}
```

**Q2.3.** True or False: `if (a < 0 && a > 10)` correctly detects "a is out of the range 0–10."
Explain your answer.

---

## Section 3 — Classes / OOP

**Q3.1.** Write a class `Driver` with a private `String name` and private `int age`, a constructor,
getters and setters for both, and a `printDriver()` method.

**Q3.2.** What is the difference between:
```java
private String name;
```
and
```java
public String name;
```
Why does the exam always ask for private fields?

**Q3.3.** Given the class from Q3.1, write the line of code that creates a `Driver` object named
"Alex" aged 30, and stores it in a variable called `d`.

**Q3.4.** What would happen if you wrote the constructor like this (spot the bug)?
```java
public Driver(String name, int age) {
    name = name;
    age = age;
}
```

---

## Section 4 — Arrays of Objects

**Q4.1.** Declare a global array that can hold up to 50 `Driver` objects, plus a counter variable
to track how many are currently stored.

**Q4.2.** Write code that creates a new `Driver` object and adds it to the array from Q4.1,
correctly updating the counter.

**Q4.3.** What bug would occur if you forgot to increment the counter after adding an object?

**Q4.4.** Why should the object only be created *after* a successful condition check, rather than
before it?

---

## Section 5 — Searching

**Q5.1.** Write a method `searchDrivers(int targetAge)` that searches the `Driver` array from
Q4.1 and prints the name of every driver with that age. If none are found, print
"No drivers found."

**Q5.2.** Why is looping `for (int i = 0; i < 50; i++)` dangerous if only 10 drivers have been
added so far?

**Q5.3.** What is the correct way to compare two `String` values for equality in Java?

---

## Section 6 — File I/O

**Q6.1.** What two `import` statements are typically needed to use `FileWriter` and handle its
exception?

**Q6.2.** Write a method `saveDriversToFile()` that writes every driver's name and age (one per
line, comma-separated) from the array in Q4.1 to a file called `drivers.txt`.

**Q6.3.** Why must file-writing code be wrapped in a `try/catch` block?

**Q6.4.** What happens if you forget to call `.close()` on your `FileWriter`?

---

## Section 7 — Mixed / Rapid Fire (30 seconds each)
1. What does `array.length` return for `int[] arr = new int[7];`?
2. What's the index of the last element in a 20-element array?
3. Fix the bug: `if (age = 18)`
4. What keyword do you use to prevent outside classes from directly accessing a field?
5. What does `this` refer to inside a constructor?
6. Which loop guarantees the body runs at least once?
7. What's the output of `(char)('A' + 2)`?
8. Name the exception thrown when you access an array index that's negative or too large.
9. Name the exception type usually caught when writing to a file.
10. `String a = "cat"; String b = "cat"; a.equals(b)` → true or false?

---

## Self-Study Tip
For every question above, don't just check the answer — **type the code out yourself** in a
plain text file (not an IDE) to simulate the no-IDE, no-autocomplete exam conditions you'll face
in Safe Exam Browser.
