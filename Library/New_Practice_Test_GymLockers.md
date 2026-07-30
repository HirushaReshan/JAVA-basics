# New Practice Test — Modeled on the Library-Style Task Format

**Scenario: Gym Locker Management System**

This is a brand-new scenario (not one you've seen before), but the task format is deliberately
identical in structure to your Library System test — 6 tasks, covering the same 6 skill areas,
so you can genuinely test whether you've learned the *patterns*, not just memorised specific
answers.

## Given: assumed starting code (`App.java`)
```java
import java.util.Scanner;

public class App {

    private static int[][] lockerBays = null;

    public static void main(String[] args) {
        System.out.println("Welcome to the Gym Locker System!");
        initialiseLockers();
        runMenu();
    }

    public static void initialiseLockers() {
        lockerBays = new int[2][];
        lockerBays[0] = new int[12]; // Bay A
        lockerBays[1] = new int[20]; // Bay B
    }

    public static void runMenu() {
        int option;
        boolean cont = true;
        while (cont) {
            option = getOption();
            switch (option) {
                case 0:
                    cont = false;
                    break;
                case 1:
                    rentLocker();
                    break;
                case 2:
                    releaseLocker();
                    break;
                case 3:
                    showLockers();
                    break;
                default:
                    System.out.println("Option not available.");
            }
        }
    }

    private static int getOption() {
        Scanner input = new Scanner(System.in);
        boolean valid = false;
        int option = -1;
        do {
            System.out.println();
            System.out.println("+--------------------------+");
            System.out.println("| MAIN MENU                |");
            System.out.println("+--------------------------+");
            System.out.println("| 1) Rent a locker          |");
            System.out.println("| 2) Release a locker       |");
            System.out.println("| 3) Show lockers           |");
            System.out.println("| 0) Quit                   |");
            System.out.println("+--------------------------+");
            System.out.print("Please select an option: ");
            try {
                option = input.nextInt();
                valid = true;
            } catch (Exception e) {
                System.out.println("This option not valid.");
            }
        } while (!valid);
        return option;
    }

    private static void rentLocker() {
        // (existing method — not shown, not relevant to these tasks)
    }

    private static void releaseLocker() {
        Scanner input = new Scanner(System.in);
        System.out.print("Enter bay number: ");
        int bay = input.nextInt() - 1;
        System.out.print("Enter locker number: ");
        int locker = input.nextInt() - 1;

        if (lockerBays[bay][locker] == 1) {
            lockerBays[bay][locker] = 0;
            System.out.println("Locker released successfully.");
        } else {
            System.out.println("This locker was not rented.");
        }
    }

    private static void showLockers() {
        // (existing method — not shown, not relevant to these tasks)
    }
}
```

---

## Task 1 — Add a Third Bay of Lockers
Refer to the `App.java` file above and identify the method that needs to be updated to meet the
following requirements:
- Add a third bay of lockers (Bay C).
- Bay C should contain 16 lockers, and ensure all lockers are available at the start of the
  program.

Write the complete, updated method.

## Task 2 — Validating Locker Number
In the `releaseLocker()` method, add the correct code to perform validation for the locker
number as follows:
- The locker number must be an integer. If the user enters a non-integer value, display an error
  message and redirect the user to the main menu.
- If the user enters a valid integer, apply the following validations before the code related to
  releasing a locker:
  - The locker number cannot be smaller than 1 (note the index starts at 0, not 1).
  - The locker number cannot be larger than the number of lockers in the bay selected.
  - Validation should include the new bay you added in Task 1 (Bay C).
  - Your solution should not contain hardcoded values.

Write the complete `releaseLocker()` method.

## Task 3 — Inclusion of a `Member` Class
Create a new class called `Member` (assume it will be in a separate file `Member.java`):
1. Attributes, correctly declared: `fullName`, `phoneNumber`, `membershipID` (this should be a
   number).
2. A constructor taking all 3 values as input.
3. Getters and setters: `getFullName()`/`setFullName()`, `getPhoneNumber()`/`setPhoneNumber()`,
   `getMembershipID()`/`setMembershipID()`.
4. A `void` method `printMember()` that prints all 3 attributes.
5. Ensure the class follows the encapsulation principle, and include a brief comment at the end
   of the class explaining how encapsulation has been applied.

Write the complete `Member` class.

## Task 4 — `Member` Objects Array
In `App.java`, create a new standard array of objects of type `Member` as a global variable (do
not use `ArrayList`), initialised with a size of 50.

## Task 5 — `registerMember()` Method
1. Create a new `void` method called `registerMember()`.
2. Prompt the user to enter member details (full name, phone number, membership ID).
3. Create a `Member` object to store the entered details.
4. Add the new object to the array from Task 4. If the array is full, print a message informing
   the user.

Write the complete `registerMember()` method.

## Task 6 — Wiring `registerMember()` Into the Menu
1. Update the printed menu in `getOption()` to include a new option, "Register member," available
   as option number 4. Write only the single new line needed.
2. Update `runMenu()` to call `registerMember()` when option 4 is selected. Write the complete
   `runMenu()` method.

---

## How to use this practice test
1. Attempt all 6 tasks **without** looking at your Library System answers first — the goal is to
   check you can apply the *pattern* to new field names/numbers, not recall memorised text.
2. Pay special attention to Task 2 (no hardcoded values — this means using `.length`, not typing
   `12`, `20`, or `16` anywhere in your validation) and Task 5 (no counter was given in Task 4, so
   you need the null-slot-search pattern, exactly like `addStaff()`).
3. Check your answers against `New_Practice_Test_Answers.md`.
4. Time yourself — aim to complete all 6 tasks within about 45-50 minutes (roughly half of a full
   90-minute exam, since this is a slightly smaller task set) to build genuine exam speed.
