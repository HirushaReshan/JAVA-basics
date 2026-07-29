# Java From Absolute Zero — Part 1: The Very Basics

This assumes you have **never written a line of code before**. Every single word is explained.
Nothing is assumed. Read this slowly, and type every example out yourself.

---

## 1. What even is a "program"?

A computer only understands electrical signals (on/off, 1s and 0s). A **programming language**
like Java is a way for *humans* to write instructions in something closer to English, which then
gets translated into something the computer can actually run.

- You write code in a file ending in `.java` (this is called your **source code**).
- A program called the **Java Compiler** reads your `.java` file and translates it into a format
  called **bytecode** (a `.class` file).
- Another program called the **JVM (Java Virtual Machine)** then runs that bytecode.

You don't need to memorise "JVM/compiler" details for the exam — you just need to know that
**writing correct Java syntax matters**, because the compiler is extremely strict: one missing
semicolon or bracket, and it refuses to run your code at all.

**IntelliJ IDEA** is the program (an "IDE" — Integrated Development Environment) you use to write,
compile, and run Java code, all in one window, with a Run button.

---

## 2. The smallest possible Java program, explained word by word

```java
public class App {
    public static void main(String[] args) {
        System.out.println("Hello, world!");
    }
}
```

Let's break down **every single word**:

### `public class App {`
| Word | What it means |
|---|---|
| `public` | An **access modifier**. It means "this class can be seen/used from anywhere, by any other code." (More on access modifiers in Part 5.) |
| `class` | A keyword that means "I am about to define a **class**." A class is a *container* — a blueprint that groups together related code and data. Think of it like a folder or a template. |
| `App` | The **name** you chose for this class. In Java, the file must be named exactly `App.java` if the class is called `App` (capital letter by convention). |
| `{` | An **opening curly brace**. Everything belonging to this class goes between this `{` and its matching `}` at the very end. Think of curly braces as a box — whatever is "inside the box" belongs to whatever came right before the `{`. |

### `public static void main(String[] args) {`
This is the **main method** — the exact starting point of every Java program. When you click
"Run," Java looks for a method that looks *exactly* like this line, and starts executing there.

| Word | What it means |
|---|---|
| `public` | Same as above — accessible from anywhere. `main` must always be `public` so Java itself can call it from outside the class. |
| `static` | Means this belongs to the **class itself**, not to any specific object made from the class. (Full explanation of `static` in Part 3 — for now: `main` is always `static`, no exceptions.) |
| `void` | The **return type**. `void` means "this method does not give back/return any value when it finishes." (Explained fully in Part 3.) |
| `main` | The **name of the method**. Java specifically looks for a method called `main` to start the program. |
| `(String[] args)` | The **parameter list** — the information this method accepts as input when called. `String[] args` means "an array of Strings, called `args`." For the exam, you basically never touch this — it exists so a program *could* accept command-line arguments, which is a rarely-used advanced feature. |
| `{` | Opens the "box" for everything the `main` method will do. |

### `System.out.println("Hello, world!");`
| Word | What it means |
|---|---|
| `System` | A built-in Java class that represents the *system* your program runs on — things like input, output, memory. |
| `.out` | A field inside `System` that represents "the standard output" — normally, your console/terminal window. |
| `.println(...)` | A **method** (`println` = "print line") that takes whatever is inside the brackets and **prints it to the screen, then moves to a new line** afterward. |
| `"Hello, world!"` | A **String literal** — literal text, always wrapped in double quotes `" "`. This is the actual data being printed. |
| `;` | A **semicolon**. Every single statement (instruction) in Java **must** end with a semicolon. Forgetting this is the #1 most common beginner error — the compiler will refuse to run your code. |

### The closing braces `}`
Each `{` must have a matching `}`. In the example:
- The first `}` closes the `main` method.
- The second `}` closes the `App` class.

**Rule of thumb:** braces must always be balanced — count your opens and closes. Indentation
(the spaces/tabs at the start of each line) doesn't affect whether the code runs, but it makes it
*readable* to humans, and is expected in the exam for good style.

---

## 3. Comments

Comments are notes for humans that Java **completely ignores** when running the program.

```java
// This is a single-line comment — everything after // on this line is ignored

/*
  This is a
  multi-line comment.
  Everything between /* and *​/ is ignored.
*/
```

Use comments to explain *why* your code does something — very useful in the exam to show the
examiner you understood the task, even under time pressure.

---

## 4. Variables and Data Types

A **variable** is a named container that stores a piece of data in the computer's memory, so you
can use and reuse it later.

### Declaring a variable
```java
int age;        // declares a variable called "age" that will hold a whole number
age = 21;       // assigns the value 21 into that variable

int score = 100; // declare AND assign in one line (most common style)
```

### Why do we need "types"?
Java is a **strongly typed** language: every variable must be given a **type** when it's created,
and that type never changes. This lets Java check your code for mistakes before it even runs
(e.g. it will refuse to let you put text into a variable meant for numbers).

### The core data types you need

| Type | Stores | Example | Notes |
|---|---|---|---|
| `int` | Whole numbers | `int rowCount = 4;` | No decimals. Range: about -2 billion to +2 billion. |
| `double` | Decimal numbers | `double fee = 4.50;` | Used for anything with a fractional part, like money. |
| `boolean` | `true` or `false` only | `boolean isOccupied = false;` | Used for yes/no, on/off logic. |
| `char` | A **single** character | `char grade = 'A';` | Single quotes `'A'`, NOT double quotes. |
| `String` | Text (a sequence of characters) | `String name = "Alex";` | Double quotes `"Alex"`. Technically a *class*, not a primitive type — more on this later. |

**Naming rule:** variable names conventionally start with a lowercase letter, and use
"camelCase" for multiple words: `parkingSlots`, `ticketCount`, `registrationNumber`.

### Assigning and reassigning
```java
int total = 0;      // total starts at 0
total = total + 5;   // total is now 5 (read the right side first, then store it back into total)
total = total + 5;   // total is now 10
total += 5;           // shorthand for "total = total + 5" — total is now 15
```

### Constants (values that never change)
```java
final int MAX_ROWS = 4;
```
`final` means this variable can be assigned **once**, and never reassigned again. By convention,
constants are written in ALL_CAPS with underscores. You won't be tested heavily on this, but it's
good to recognise.

---

## 5. Operators

### Arithmetic operators
```java
int a = 10, b = 3;
System.out.println(a + b); // 13  (addition)
System.out.println(a - b); // 7   (subtraction)
System.out.println(a * b); // 30  (multiplication)
System.out.println(a / b); // 3   (division — integer division truncates decimals!)
System.out.println(a % b); // 1   (modulus — the REMAINDER after division)
```
⚠️ **Integer division trap:** `10 / 3` in Java gives `3`, not `3.333...`, because both `a` and
`b` are `int`. To get a decimal result, at least one side needs to be a `double`:
```java
double result = (double) a / b; // 3.333333...
```
`(double)` here is called a **cast** — it temporarily treats `a` as a `double` for this
calculation.

### Comparison operators (always give back `true` or `false`)
```java
a == b   // equal to
a != b   // not equal to
a > b    // greater than
a < b    // less than
a >= b   // greater than or equal to
a <= b   // less than or equal to
```
⚠️ `=` is **assignment** ("put this value into that variable"). `==` is **comparison** ("are
these two values equal?"). Mixing these up is one of the most common beginner bugs.

### Logical operators (combine multiple true/false conditions)
```java
&&   // AND — both sides must be true
||   // OR  — at least one side must be true
!    // NOT — flips true to false, and false to true
```
```java
int row = 2;
if (row >= 0 && row <= 3) {
    System.out.println("Row is valid.");
}
```

---

## 6. Getting input from the user (`Scanner`)

```java
import java.util.Scanner; // tells Java "I want to use the Scanner tool from its library"

Scanner input = new Scanner(System.in); // creates a Scanner that reads from the keyboard

int age = input.nextInt();       // reads one whole number typed by the user
double price = input.nextDouble(); // reads one decimal number
String line = input.nextLine();    // reads an entire line of text (until Enter is pressed)
```

- `import java.util.Scanner;` must go at the **very top** of the file, before `public class App`.
  It tells Java "bring in this pre-built tool so I can use it here."
- `new Scanner(System.in)` **creates** a Scanner object that watches `System.in` (the keyboard).
  We'll fully explain what `new` and "creating an object" means in Part 5 — for now, just know
  this line is boilerplate you always write to accept input.
- Each `next___()` method **waits** for the user to type something and press Enter, then gives you
  that value.

---

## Quick Self-Test
1. What is the difference between `=` and `==`?
2. Why does `int` not store decimal numbers?
3. What symbol must end every Java statement?
4. What do curly braces `{ }` represent?
5. Why do we write `import java.util.Scanner;` at the top of a file?

*(Answers are provided in the final answers file for this whole series.)*

**Next: Part 2 — Control Flow (if/else, switch, loops)**
