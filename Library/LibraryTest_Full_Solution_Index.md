# Library System — Full Assembled Solution & Index

## Index — where to find each explanation

| Task | File | What it teaches |
|---|---|---|
| Task 1 | `LibraryTest_Task1_Explained.md` | Adding a row to a jagged array — `initialiseShelves()` |
| Task 2 | `LibraryTest_Task2_Explained.md` | Integer validation (`try/catch`) + range validation without hardcoding, using `.length` |
| Task 3 | `LibraryTest_Task3_Explained.md` | The `Staff` class — fields, constructor, custom-named getters/setters, encapsulation comment |
| Task 4 | `LibraryTest_Task4_Explained.md` | Declaring a global array of objects (no `ArrayList`) |
| Task 5 | `LibraryTest_Task5_Explained.md` | Adding to an array **without** a counter — searching for `null` slots |
| Task 6 | `LibraryTest_Task6_Explained.md` | Wiring a new method into the menu — `getOption()` + `runMenu()` |

---

## `Staff.java` (Task 3)

```java
public class Staff {

    private String name;
    private String surname;
    private int staffID;

    public Staff(String name, String surname, int staffID) {
        this.name = name;
        this.surname = surname;
        this.staffID = staffID;
    }

    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }

    public String getSurname() {
        return surname;
    }
    public void setSurname(String surname) {
        this.surname = surname;
    }

    public int getID() {
        return staffID;
    }
    public void setID(int staffID) {
        this.staffID = staffID;
    }

    public void printStaff() {
        System.out.println("Name: " + name + ", Surname: " + surname + ", Staff ID: " + staffID);
    }

    // Encapsulation has been applied by declaring all attributes (name, surname, staffID) as
    // private, so they cannot be accessed directly from outside this class. Access is only
    // possible through the public getter and setter methods, which control how the data is
    // read or modified.
}
```

## `App.java` (Tasks 1, 2, 4, 5, 6 combined — my best reconstruction)

```java
import java.util.Scanner;

public class App {

    private static int[][] bookShelves = null;

    // Task 4
    private static Staff[] staffMembers = new Staff[100];

    public static void main(String[] args) {
        System.out.println("Welcome to the Library System!");
        initialiseShelves();
        runMenu();
    }

    // Task 1
    public static void initialiseShelves() {
        bookShelves = new int[3][];
        bookShelves[0] = new int[10]; // Shelf 1
        bookShelves[1] = new int[15]; // Shelf 2
        bookShelves[2] = new int[10]; // Shelf 3
    }

    // Task 6, part 2
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
                    borrowBook();
                    break;
                case 2:
                    returnBook();
                    break;
                case 3:
                    showShelves();
                    break;
                case 4:
                    addStaff();
                    break;
                default:
                    System.out.println("Option not available.");
            }
        }
    }

    // Task 6, part 1 (the "| 4) Add staff |" line is added inside this method)
    private static int getOption() {
        Scanner input = new Scanner(System.in);
        boolean valid = false;
        int option = -1;
        do {
            System.out.println();
            System.out.println("+--------------------------+");
            System.out.println("| MAIN MENU                |");
            System.out.println("+--------------------------+");
            System.out.println("| 1) Borrow a book          |");
            System.out.println("| 2) Return a book          |");
            System.out.println("| 3) Show shelves           |");
            System.out.println("| 4) Add staff              |");
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

    private static void borrowBook() {
        // Assumed pre-existing method, same pattern as returnBook() but marking a book as
        // borrowed (1) instead of returned (0). Not part of the given tasks.
    }

    // Task 2
    private static void returnBook() {
        Scanner input = new Scanner(System.in);
        System.out.print("Enter shelf number: ");
        int shelf = input.nextInt() - 1;

        System.out.print("Enter book number: ");
        int book;
        try {
            book = input.nextInt();
        } catch (Exception e) {
            System.out.println("Invalid input. Book number must be an integer.");
            return;
        }
        book = book - 1;

        if (book < 0 || book >= bookShelves[shelf].length) {
            System.out.println("Invalid book number.");
        } else if (bookShelves[shelf][book] == 1) {
            bookShelves[shelf][book] = 0;
            System.out.println("Book returned successfully.");
        } else {
            System.out.println("This book was not borrowed.");
        }
    }

    private static void showShelves() {
        // Assumed pre-existing display method, same pattern as showSeatingArea()/
        // showParkingLayout() from your other two projects. Not part of the given tasks.
    }

    // Task 5
    private static void addStaff() {
        Scanner input = new Scanner(System.in);

        System.out.print("Enter staff name: ");
        String name = input.nextLine();

        System.out.print("Enter staff surname: ");
        String surname = input.nextLine();

        System.out.print("Enter staff ID: ");
        int staffID = input.nextInt();

        Staff staff = new Staff(name, surname, staffID);

        boolean added = false;

        for (int i = 0; i < staffMembers.length; i++) {
            if (staffMembers[i] == null) {
                staffMembers[i] = staff;
                added = true;
                break;
            }
        }

        if (added) {
            System.out.println("Staff added successfully.");
        } else {
            System.out.println("Staff list is full. Cannot add more staff.");
        }
    }
}
```

⚠️ Again: `borrowBook()` and `showShelves()` are **assumed placeholders**, since they weren't
given to you and weren't part of any task — in your real exam, these will already exist in the
provided code, so you won't need to write them yourself unless a question specifically asks.

---

## The 6 Skill Areas This Test Confirms (compare to your Plane App / Car Park mock tests)

| Skill | Where tested here | Where tested before |
|---|---|---|
| Extending a jagged array (adding a row) | Task 1 | Plane App / Car Park Q1 (resizing rows) |
| Input validation, including non-integer handling | Task 2 | Plane App / Car Park Q2 (range only — this adds `try/catch` on top) |
| Writing a class with fields, constructor, getters/setters | Task 3 | Plane App / Car Park Q3 |
| Declaring a global array of objects | Task 4 | Plane App / Car Park Q4 (part 1) |
| Adding objects to an array (this time via null-search, not a counter) | Task 5 | Plane App / Car Park Q4 (part 2) |
| Wiring new functionality into the menu | Task 6 (new!) | Not directly tested before — genuinely new skill |

**Task 6 is the one genuinely new skill** compared to your other mock tests — editing
`getOption()`'s printed menu and `runMenu()`'s `switch` statement to actually make a new feature
reachable by the user. Make sure you're comfortable with this pattern, since it could easily
appear in your real exam too, especially if a new method like `searchPayments()` or `saveToFile()`
needs to be reachable from the menu.
