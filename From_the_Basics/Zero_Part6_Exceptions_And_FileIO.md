# Java From Absolute Zero  Part 6: Exceptions & File I/O

## 1. What is an exception?

An **exception** is Java's way of saying "something went wrong while running this code, and I
don't know how to continue normally." If left unhandled, it **crashes your program** and prints
an error message (a "stack trace").

Examples you've already seen the causes of:
- `ArrayIndexOutOfBoundsException`  accessed an array index that doesn't exist.
- `NullPointerException`  tried to use an object that is actually `null` (empty/not created yet).
- `InputMismatchException`  `Scanner` expected a number (`nextInt()`) but the user typed letters.
- `IOException`  something went wrong reading/writing a file (e.g. disk full, file locked).

---

## 2. `try` / `catch`  handling exceptions gracefully

```java
try {
    int result = 10 / 0; // this would normally crash
} catch (ArithmeticException e) {
    System.out.println("You can't divide by zero!");
}
```
- `try { }`  "attempt to run this code. If anything inside throws an exception, stop
  immediately and jump to the matching `catch` block below, instead of crashing."
- `catch (ArithmeticException e) { }`  "if the exception thrown matches this type, run this
  recovery code instead." `e` is just a variable name (you can call it anything) representing
  the exception object itself, which contains details about what went wrong.

### Real example from your project  `getOption()`
```java
try {
    option = input.nextInt();
    valid = true;
} catch (Exception e) {
    System.out.println("This option is not valid.");
    input.nextLine();
}
```
If the user types a word instead of a number, `input.nextInt()` throws an exception. Instead of
crashing, the `catch` block runs: it prints a friendly message and clears the bad input
(`input.nextLine()`) so the loop can safely ask again.

**`Exception`** (used here) is a very general "catch anything" type  it will catch almost any
kind of exception. More specific types like `ArithmeticException` or `IOException` only catch
that specific kind  useful when you want different handling for different problems.

### Checked vs Unchecked exceptions (just enough to get by)
- **Checked exceptions** (e.g. `IOException`)  Java **forces** you to either `catch` them or
  declare `throws IOException` on the method. This is why file-writing code must always be
  wrapped in `try/catch`.
- **Unchecked exceptions** (e.g. `ArrayIndexOutOfBoundsException`, `NullPointerException`)  Java
  does **not** force you to handle these; if unhandled, they simply crash the program at runtime.
  This is exactly why you write **validation code** (Part 2 style `if` checks) to *prevent* these
  from ever happening, rather than catching them after the fact.

---

## 3. File I/O  reading and writing text files

"I/O" stands for **Input/Output**  I in this context means reading from a file, O means writing
to a file.

### Writing to a file with `FileWriter`
```java
import java.io.FileWriter;
import java.io.IOException;

private static void saveToFile() {
    try {
        FileWriter writer = new FileWriter("tickets.txt");

        writer.write("Hello, this is a line in the file.");
        writer.write(System.lineSeparator()); // moves to a new line

        writer.close();
        System.out.println("Saved successfully.");

    } catch (IOException e) {
        System.out.println("An error occurred while saving.");
    }
}
```

Word by word:
- `import java.io.FileWriter;` / `import java.io.IOException;`  bring in Java's built-in tools
  for writing files and handling file-related errors. Without these imports, `FileWriter` and
  `IOException` wouldn't be recognised.
- `new FileWriter("tickets.txt")`  creates a new writer that's ready to write to a file called
  `tickets.txt` (created automatically if it doesn't exist, in your project's folder). This
  operation is **risky** (the disk could fail, permissions could be wrong, etc.), which is why
  it must be inside a `try` block, and why declaring/using it can throw `IOException`.
- `writer.write("...")`  writes the given text into the file (does **not** automatically add a
  new line  unlike `println`).
- `System.lineSeparator()`  a safe, cross-platform way to insert "start a new line" into the
  file (better than hardcoding `"\n"`, since Windows/Mac/Linux technically use slightly different
  line-ending characters).
- `writer.close()`  **very important**: finishes writing and releases the file so other
  programs can use it. Forgetting this can leave data un-saved or the file locked.
- `catch (IOException e)`  catches any file-related error, so your whole program doesn't crash
  just because saving failed.

### Writing multiple records (looping while writing  this is exactly your exam's Question 6 pattern)
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
For every object currently stored (looping `0` to `ticketCount - 1`, exactly like the searching
pattern from earlier), write one line containing that object's data, separated by a comma, then
move to a new line before writing the next record.

### Alternative: `PrintWriter` (has a convenient `println` method, just like `System.out`)
```java
import java.io.PrintWriter;
import java.io.FileNotFoundException;

private static void saveToFile() {
    try {
        PrintWriter writer = new PrintWriter("tickets.txt");

        for (int i = 0; i < ticketCount; i++) {
            writer.println(tickets[i].getRegistrationNumber() + "," + tickets[i].getParkingFee());
        }

        writer.close();
        System.out.println("Saved successfully.");

    } catch (FileNotFoundException e) {
        System.out.println("An error occurred while saving.");
    }
}
```
`writer.println(...)` automatically adds a new line for you, so you don't need the separate
`System.lineSeparator()` call. Note the different exception type here (`FileNotFoundException`
instead of `IOException`)  this is a detail of which class throws what; either approach is
correct as long as the exception type you catch actually matches what the writer class can throw.

---

## Quick Self-Test
1. What is the difference between a checked and unchecked exception?
2. Why must file-writing code always be inside a `try/catch`?
3. What does `writer.close()` do, and why does forgetting it matter?
4. What's the difference between `.write()` and `.println()` on a writer, in terms of new lines?
5. What two `import` statements do you need for the `FileWriter` + `IOException` version of
   saving to a file?

---

## You've now covered every fundamental needed for the exam
At this point you understand, from zero: program structure, variables/types, operators, control
flow (if/switch/loops), methods (return types, parameters, static, access modifiers), arrays
(1D/2D/jagged, arrays of objects), full OOP (classes, objects, constructors, this, getters,
setters, encapsulation, static vs instance), exceptions, and file I/O.

**Next step:** go to `[2_Concept_Teaching_Full.md](../Lab_content/2_Concept_Teaching_Full,md)` and `[3_CarParkManagement_Deep_Dive.md](../Lab_content/3_CarParkManagement_Deep_Dive.md)`
Then move on to the practice
questions and the timed mock test.
