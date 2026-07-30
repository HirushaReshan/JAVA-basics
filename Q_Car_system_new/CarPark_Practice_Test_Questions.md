# Car Park Practice Test — Modeled on the Library-Style Task Format

This uses your **actual, real** Car Park `App.java` (from `CarParkManagement_App_Code_Reference.pdf`)
as the starting point — not a reconstruction. Same 6-task structure and skill areas as your
Library System test, including the two newer skills (non-hardcoded integer+range validation with
`try/catch`, and the null-slot-search add pattern), applied to your actual project.

## Starting point: the real given code (relevant excerpts)
```java
001: import java.util.Scanner;
...
006: private static int[][] parkingSlots = null;
...
014: public static void initialiseParkingSlots() {
015:     parkingSlots = new int[4][];
016:     parkingSlots[0] = new int[18];
017:     parkingSlots[1] = new int[20];
018:     parkingSlots[2] = new int[20];
019:     parkingSlots[3] = new int[18];
020: }
...
074: private static void parkCar() {
075:     Scanner input = new Scanner(System.in);
076:     System.out.print("Enter parking row (1-4): ");
077:     int row = input.nextInt() - 1;
078:     System.out.print("Enter parking slot: ");
079:     int slot = input.nextInt() - 1;
080:
081:     if (parkingSlots[row][slot] == 0) {
082:         parkingSlots[row][slot] = 1;
083:         System.out.println("Vehicle parked successfully.");
084:         showParkingLayout();
085:     } else {
086:         System.out.println("This parking slot is already occupied.");
087:     }
088: }
...
046: private static int getOption() {
        // ... prints the MAIN MENU box with:
        // | 1) Park a car
        // | 2) Show parking layout
        // | 0) Quit
...
022: public static void runMenu() {
        // ... switch: case 0 quit, case 1 parkCar(), case 2 showParkingLayout(), default
...
```

---

## Task 1 — Add an Additional Row of Parking (Row E)
Refer to the `App.java` file and identify the method that needs to be updated to meet the
following requirements:
- Add a fifth parking row (Row E).
- Row E should contain 14 parking slots, and ensure all slots are available at the start of the
  program.

Write the complete, updated method.

## Task 2 — Validating Parking Slot Number
In the `parkCar()` method, add the correct code to perform validation for the **slot number** as
follows:
- The slot number must be an integer. If the user enters a non-integer value, display an error
  message and redirect the user to the main menu.
- If the user enters a valid integer, apply the following validations before the code related to
  parking a car:
  - The slot number cannot be smaller than 1 (note the index starts at 0, not 1).
  - The slot number cannot be larger than the number of slots in the row selected.
  - Validation should include the new row you added in Task 1 (Row E).
  - Your solution should not contain hardcoded values.

Write the complete `parkCar()` method.

## Task 3 — Inclusion of an `Attendant` Class
Create a new class called `Attendant` (assume it will be in a separate file `Attendant.java`):
1. Attributes, correctly declared: `firstName`, `lastName`, `attendantID` — this should be a
   number.
2. A constructor taking all 3 values as input.
3. Getters and setters: `getFirstName()`/`setFirstName()`, `getLastName()`/`setLastName()`,
   `getID()`/`setID()`.
4. A `void` method `printAttendant()` that prints all 3 attributes.
5. Ensure the class follows the encapsulation principle, and include a brief comment at the end
   of the class explaining how encapsulation has been applied.

Write the complete `Attendant` class.

## Task 4 — `Attendant` Objects Array
In `App.java`, create a new standard array of objects of type `Attendant` as a global variable
(do not use `ArrayList`), initialised with a size of 100.

## Task 5 — `addAttendant()` Method Creation
1. Create a new `void` method called `addAttendant()`.
2. Prompt the user to enter attendant details (first name, last name, attendant ID).
3. Create an `Attendant` object to store the entered details.
4. Add the new object to the array from Task 4. If the array is full, print a message informing
   the user.

Write the complete `addAttendant()` method.

## Task 6 — Wiring `addAttendant()` Into the Menu
1. Update the printed menu in `getOption()` to include a new option, "Add attendant," available
   as option number 3 (note: option 3 is free in this project, since only 1 and 2 are currently
   used). Write only the single new line needed.
2. Update `runMenu()` to call `addAttendant()` when option 3 is selected. Write the complete
   `runMenu()` method.

---

## Notice what's deliberately different from your Library System test
- **Option number changed** (option 3, not 4) — because Car Park's menu only currently has
  options 1 and 2 used, unlike the Library's 1-2-3. Always check the *actual* existing menu
  before assuming which number is free — don't just copy "4" out of habit.
- **`getID()`/`setID()` naming is the same trap as before** — the field is `attendantID`, but the
  method names specified are the shorter `getID()`/`setID()`. Watch for this every time; don't
  assume it'll always be the shortened version either — always read what's actually asked.
- **Row E's slot count (14) doesn't match any existing row** — this is intentional, to make sure
  you're not pattern-matching a specific number, but genuinely reading the requirement.

## How to use this practice test
1. Attempt all 6 tasks cold, without re-reading your Library System or Gym Locker answers first.
2. Since this is your **actual real project**, once you're confident in your written answers,
   open the real `4COSC010C_CarParkManagement` project in IntelliJ, apply your changes for real,
   compile it, and test it by actually running the program — park cars, trigger the invalid-input
   error path, add attendants until the array fills up, and check the menu displays and works.
3. Check your written answers against `CarPark_Practice_Test_Answers.md` before you test in
   IntelliJ, then use IntelliJ itself as the final check — if it compiles and behaves correctly,
   you know your answer was genuinely right, not just "looked right on paper."
