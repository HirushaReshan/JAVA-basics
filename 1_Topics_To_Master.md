# 4COSC010C Lab Practical  Topics You Must Master
### (Based on the Mock Test structure, applied to Car Park Management)

## How the real test is structured
Your Final Lab Practical will be **6 questions**, following the exact same *shape* as the Mock
(Plane App) test, but built around the **Car Park Management** case study instead. The numbers
and wording will change, but the underlying Java skill being tested in each question will not.

Here is the mapping (Mock → what it tests → what the real test will likely test):

| # | Mock Question (Plane App) | Java Skill Being Tested | Car Park Equivalent (most likely) |
|---|---|---|---|
| 1 | Rewrite `initialiseRows()` with new seat counts per row | **2D / jagged arrays**, array initialisation | Rewrite `initialiseParkingSlots()` with new slot counts per row |
| 2 | Add validation to `buyTicket()` for row bounds | **Input validation**, `if/else`, boolean logic, loops for re-prompting | Add validation to `parkCar()` for row/slot bounds |
| 3 | Create a `Payment` class: fields, constructor, getters/setters, print method | **OOP**: classes, encapsulation, constructors, accessor methods | Create a class (e.g. `Ticket`/`Vehicle`) with similar structure |
| 4 | Array of `Payment` objects + updated `buyTicket()` that creates objects conditionally, with pricing logic | **Arrays of objects**, conditional object creation, calculations | Array of `Ticket` objects + updated `parkCar()` with a fee calculation |
| 5 | `searchPayments()`  search array of objects by amount, print matches or "not found" | **Searching/iterating object arrays**, flags, string/number comparison | `searchTickets()`  search by fee or registration number |
| 6 | `saveToFile()`  write every payment to a text file, one line per record | **File I/O** (`FileWriter`/`PrintWriter`), exception handling | `saveToFile()`  write every ticket to a text file |

**Bottom line:** the 6 questions test 6 core skill areas. If you can comfortably do all 6 with
the Car Park project as practice, you can do it with whatever exact numbers/fields they give you
on the day.

---

## The 6 Skill Areas (study checklist)

### ✅ Skill 1  Arrays (1D, 2D / jagged arrays)
- Declaring `int[][] name` vs `int[] name`
- Jagged arrays: each row can have a **different length** (`new int[4][]` then assign each row separately)
- Array indexing starts at **0**, but user-facing numbering usually starts at 1 (row 1 → index 0)
- Rewriting an **entire method** cleanly and completely (not just a snippet)

### ✅ Skill 2  Input Validation & Control Flow
- `if / else if / else`
- Comparison operators (`<`, `>`, `<=`, `>=`, `==`, `!=`)
- Logical operators (`&&`, `||`, `!`)
- Loops for **re-prompting** until valid input is given (`while`, `do-while`)
- Combining bounds-checking (row/slot in range) with the existing occupied/available check

### ✅ Skill 3  Object-Oriented Programming (classes)
- Declaring a class in its own file
- **Private fields** (encapsulation) + **public getters/setters**
- Writing a **constructor** that takes parameters and assigns `this.field = param`
- Writing an instance **print method** using `System.out.println`
- Difference between a **class** (blueprint) and an **object** (instance)

### ✅ Skill 4  Arrays of Objects + Conditional Object Creation
- Declaring `Ticket[] tickets = new Ticket[100];` as a **global/static** variable
- Keeping a **counter** of how many objects have been added so far
- Only creating/storing a new object **if a condition succeeds** (e.g. parking succeeded)
- Doing a **calculation** (e.g. price based on row) before creating the object
- Order of operations: collect input → validate → check availability → calculate → create object → store → display result

### ✅ Skill 5  Searching Arrays of Objects
- Looping through an array of objects up to the **actual count** used (not the full capacity)
- Comparing a field of each object to a target value
- Using a **boolean "found" flag** to detect the no-match case
- Printing "not found" messages correctly

### ✅ Skill 6  File I/O
- `FileWriter`, `BufferedWriter`, or `PrintWriter`
- Wrapping file operations in `try { } catch (IOException e) { }`
- Writing one line per object using `write()`/`println()`
- Closing the writer (or using try-with-resources)
- Formatting each line consistently (e.g. `email,amount` or `reg,fee`)

---

## Supporting Java Fundamentals (Week 1–10 revision list)
Even though the 6 questions focus on the areas above, make sure these fundamentals are rock
solid, because they show up **inside** every question:

1. **Variables & data types**: `int`, `double`, `String`, `char`, `boolean`
2. **Scanner input**: `nextInt()`, `nextLine()`, `next()`, and the classic "leftover newline" bug
3. **String methods**: `.equals()`, `.equalsIgnoreCase()`, concatenation with `+`
4. **char arithmetic**: how `(char)('A' + row)` works (used in `showParkingLayout()`)
5. **Static vs instance**: why everything in `App.java` is `static`, but `Ticket` methods are not
6. **Switch statements**: `runMenu()`'s use of `switch/case/break/default`
7. **Methods**: return types, parameters, `void` vs returning a value, public vs private
8. **Exception handling basics**: `try/catch`, why `getOption()` catches input errors
9. **`this` keyword**: used inside constructors and setters to distinguish field from parameter
10. **Loops**: `for`, `while`, `do-while`, and knowing when to use each

---

## Suggested Study Order
1. Read **`2_Concept_Teaching_Full.md`**  learn every concept from zero, with examples.
2. Read **`3_CarParkManagement_Deep_Dive.md`**  see these concepts applied line-by-line to the
   actual Car Park project, with fully worked solutions for all 6 skill areas.
3. Do **`4_Practice_And_Test_Questions.md`**  smaller drills to test your understanding of each
   isolated concept.
4. Sit **`5_Full_Practice_Lab_Test.md`** under timed conditions (90 minutes, no IDE, handwritten/typed
   in a plain text editor  simulate the real Safe Exam Browser experience).
5. Mark yourself using **`6_Answers_And_Cheat_Sheet.md`**, then review anything you got wrong.
