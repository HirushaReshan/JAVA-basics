# Mock Test — Full Assembled Solution & Index

Use this file to see how **all 6 questions fit together** into one complete, working program,
and as a map back to the detailed word-by-word explanation of each question.

---

## Index — where to find each explanation

| Question | File | What it teaches |
|---|---|---|
| Q1 (5 pts) | `MockTest_Q1_Explained.md` | Rewriting `initialiseRows()` — jagged arrays |
| Q2 (15 pts) | `MockTest_Q2_Explained.md` | Adding validation to `buyTicket()` — if/else chains, index translation |
| Q3 (20 pts) | `MockTest_Q3_Explained.md` | Writing the `Payment` class — fields, constructor, getters/setters, `this` |
| Q4 (20 pts) | `MockTest_Q4_Explained.md` | Array of objects + conditional object creation + pricing logic |
| Q5 (30 pts) | `MockTest_Q5_Explained.md` | Searching an array of objects — loops, flags, `.equals()` vs `==` |
| Q6 (10 pts) | `MockTest_Q6_Explained.md` | Saving objects to a file — `try/catch`, `FileWriter` |

If any single line below doesn't make sense, go to the matching file above — it explains that
exact line, word by word, and why it exists.

---

## The complete `Payment.java` (from Question 3)

```java
public class Payment {

    private String email;
    private double paymentAmount;

    public Payment(String email, double paymentAmount) {
        this.email = email;
        this.paymentAmount = paymentAmount;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public double getPaymentAmount() {
        return paymentAmount;
    }

    public void setPaymentAmount(double paymentAmount) {
        this.paymentAmount = paymentAmount;
    }

    public void printPayment() {
        System.out.println("Email: " + email + ", Payment Amount: " + paymentAmount);
    }
}
```

---

## The complete, fully updated `App.java` (Questions 1, 2, 4, 5, 6 combined)

This shows how every answer stacks on top of the original given code, in the order you'd actually
build it during the real exam.

```java
import java.util.Scanner;
import java.io.FileWriter;
import java.io.IOException;

public class App {

    // ---- Original global variables (given) ----
    private static int[][] planeSeats = null;
    private static int[] pricePerRow = null;

    // ---- NEW global variables (Question 4, part 1) ----
    private static Payment[] payments = new Payment[100];
    private static int paymentCount = 0;

    public static void main(String[] args) {
        System.out.println("Welcome to Flying Java!");
        initialiseRows();
        runMenu();
    }

    // ---- Question 1: rewritten with new seat counts ----
    public static void initialiseRows() {
        planeSeats = new int[4][];
        planeSeats[0] = new int[16]; // row 1
        planeSeats[1] = new int[22]; // row 2
        planeSeats[2] = new int[22]; // row 3
        planeSeats[3] = new int[16]; // row 4
        pricePerRow = new int[4];
        pricePerRow[0] = 50;
        pricePerRow[1] = 80;
        pricePerRow[2] = 80;
        pricePerRow[3] = 50;
    }

    // ---- Given, unchanged ----
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
                    buyTicket();
                    break;
                case 2:
                    showSeatingArea();
                    break;
                case 3:                       // NEW menu option for Question 5
                    searchPayments();
                    break;
                case 4:                       // NEW menu option for Question 6
                    saveToFile();
                    break;
                default:
                    System.out.println("Option not available.");
            }
        }
    }

    // ---- Given, unchanged ----
    private static int getOption() {
        Scanner input = new Scanner(System.in);
        boolean valid = false;
        int option = -1;
        do {
            System.out.println();
            System.out.println("+--------------------------+");
            System.out.println("| MAIN MENU                |");
            System.out.println("+--------------------------+");
            System.out.println("| 1) Buy a plane ticket     |");
            System.out.println("| 2) Show seating area      |");
            System.out.println("| 3) Search payments        |");
            System.out.println("| 4) Save payments to file  |");
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

    // ---- Questions 2 + 4 combined ----
    private static void buyTicket() {
        Scanner input = new Scanner(System.in);

        System.out.print("Enter email address: ");
        String email = input.nextLine();

        System.out.print("Enter row number: ");
        int row = input.nextInt() - 1;
        System.out.print("Enter seat number: ");
        int seat = input.nextInt() - 1;

        if (row < 0 || row > 3) {
            System.out.println("Invalid row number.");
        } else if (planeSeats[row][seat] == 0) {
            planeSeats[row][seat] = 1;

            int amount = pricePerRow[row];
            Payment payment = new Payment(email, amount);
            payments[paymentCount] = payment;
            paymentCount++;

            System.out.println("Purchase successful. Amount: £" + amount);
            showSeatingArea();
        } else {
            System.out.println("This seat is already taken.");
        }
    }

    // ---- Question 5: brand new method ----
    private static void searchPayments() {
        Scanner input = new Scanner(System.in);
        System.out.print("Enter amount to search for: ");
        int amount = input.nextInt();

        boolean found = false;

        for (int i = 0; i < paymentCount; i++) {
            if (payments[i].getPaymentAmount() == amount) {
                System.out.println(payments[i].getEmail());
                found = true;
            }
        }

        if (!found) {
            System.out.println("No payments were found.");
        }
    }

    // ---- Question 6: brand new method ----
    private static void saveToFile() {
        try {
            FileWriter writer = new FileWriter("payments.txt");

            for (int i = 0; i < paymentCount; i++) {
                writer.write(payments[i].getEmail() + "," + payments[i].getPaymentAmount());
                writer.write(System.lineSeparator());
            }

            writer.close();
            System.out.println("Payments saved successfully.");

        } catch (IOException e) {
            System.out.println("An error occurred while saving.");
        }
    }

    // ---- Given, unchanged ----
    private static void showSeatingArea() {
        int rows = planeSeats.length;
        char aisle = '|';

        System.out.println("=".repeat(76));
        System.out.println("SEATING AREA");
        System.out.println("=".repeat(76));

        for (int row = 0; row < rows; row++) {
            System.out.print("Row " + (row + 1) + " (£" + pricePerRow[row] + ") ");
            int seatsPerRow = planeSeats[row].length;
            for (int seat = 0; seat < seatsPerRow; seat++) {
                if (seat == 9) {
                    System.out.print(" " + aisle + " ");
                }
                if (planeSeats[row][seat] == 0) {
                    System.out.print("[O]");
                } else {
                    System.out.print("[X]");
                }
            }
            System.out.println();
        }
        System.out.println("=".repeat(76));
        System.out.println("LEGEND: [O] = Seat available, [X] = Seat taken");
        System.out.println("=".repeat(76));
        System.out.println();
    }
}
```

**Note on the menu:** adding `case 3` and `case 4` to `runMenu()` and the extra lines in
`getOption()`'s printed menu are **not explicitly asked for** in the mock questions — they're
included here only so the whole program is genuinely runnable end-to-end for your own practice
in IntelliJ. In the actual exam, you only need to write exactly what each question asks for
(the method itself), not necessarily wire it into the menu, unless a question specifically tells
you to.

---

## How to use this for revision

1. **Don't just read this file.** Open IntelliJ, create the actual `4COSC010C_Mock_PlaneApp`
   project fresh, and type all of this out yourself, from memory where you can, checking back
   only when stuck.
2. Run it. Try buying a few tickets, search for payments, save to file, and open the resulting
   `payments.txt` to see it for real.
3. Then go through each `MockTest_QX_Explained.md` file's "Quick self-check" questions without
   looking at the answers first.
4. Finally, apply the exact same logic to the Car Park Management project using your earlier
   `3_CarParkManagement_Deep_Dive.md` and `5_Full_Practice_Lab_Test.md` files — the skills
   transfer directly.
