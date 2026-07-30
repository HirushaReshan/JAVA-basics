# Library System — Task 6 Explained (Wiring `addStaff()` into the Menu)

## ⚠️ Assumed original code (based on the established project pattern)
```java
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
            default:
                System.out.println("Option not available.");
        }
    }
}
```

---

## Decoding the question, sentence by sentence

> "Update the printed menu in getOption() [line no 49-60 in App.java file] to include a new
> option called 'Add staff'. The option should be available when the user selects option number
> 4."

**Plain English:** Somewhere inside `getOption()`, there's a series of `System.out.println(...)`
lines that draw the menu box on screen. You need to add **one more line** to that list, showing
"4) Add staff" as a new choice.

> "Only write the single line that needs to be added to getOption() method to satisfy the
> requirement above."

**Plain English:** They don't want the whole method rewritten here — **only** the one new
`println` line, on its own.

> "Update the runMenu() [line no 25-47 in App.java file] method to call the new function
> addStaff() from the previous task. Write the complete runMenu() method with updated code."

**Plain English:** Unlike the `getOption()` part, here they DO want the **entire method** rewritten
— add a new `case 4:` to the `switch` statement that calls `addStaff()`.

---

## Step 1 — The single line for `getOption()`

Looking at the existing menu-printing lines, you're just adding one more, styled consistently
with the others (matching the box-drawing characters and alignment):

```java
System.out.println("| 4) Add staff              |");
```

That's the entire answer to this part — literally just this one line, inserted among the other
`println` lines that draw the menu (typically right after the last option and before the closing
border line), so it visually shows up as a new choice on screen.

**Why does exact spacing/alignment matter (a little)?** The `|` characters are meant to line up
visually to form a neat box shape when printed. It won't affect whether your code *compiles or
runs correctly* if the spacing is slightly off, but matching the existing style shows attention
to detail, which examiners do notice.

---

## Step 2 — Adding the `case 4` to `runMenu()`'s switch statement

```java
case 4:
    addStaff();
    break;
```
This follows the **exact same pattern** as every other case already in the switch: a `case`
label matching the menu number, a call to the relevant method, and a `break;` to stop the switch
from falling through into the next case.

**Where in the switch should this go?** Anywhere among the other `case` blocks is technically
fine (Java doesn't require cases to be in numeric order), but for readability and to match what
an examiner expects, place it directly after `case 3` and before `default`.

---

## Step 3 — The complete final answer for `runMenu()`

```java
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
```

**Everything except the new `case 4` block is completely unchanged** from the original — this is
exactly the same "surgical addition" skill tested throughout every one of your mock questions:
find the exact right spot, add exactly what's needed, leave everything else untouched, but submit
the whole method since that's what was asked for.

---

## Why the question splits these two parts differently
Notice the question asks for **only one line** for `getOption()`, but the **complete method** for
`runMenu()`. This isn't inconsistent — it's testing whether you read instructions carefully and
follow the *specific* format requested for each individual part, rather than assuming every part
of a multi-part question wants the same thing. Always re-read each sub-instruction's formatting
requirement separately.

---

## Quick self-check
1. Why does this task ask for just one line for `getOption()`, but the whole method for
   `runMenu()`?
2. What would happen if you forgot the `break;` after `addStaff();` inside `case 4`?
3. Why doesn't the position of `case 4` within the switch block matter for correctness (only for
   readability)?
4. What's the risk of copying the wrong menu-line format for `getOption()` (e.g. missing the `|`
   characters)?

*(1: because the question explicitly specifies different requirements for each part — always
follow the exact formatting instruction given for each individual sub-task rather than assuming
consistency. 2: execution would "fall through" into whatever code comes after `case 4` in the
switch (likely `default`), potentially running unintended extra code/printing an unwanted
message immediately after `addStaff()` runs. 3: because `switch` statements check the value
against each `case` independently — Java doesn't require or expect them to be listed in numeric
order for the program to work correctly. 4: it would still *work* correctly (the option would
still be selectable and functional), but the printed menu would look visually broken/misaligned
compared to the other lines, which could cost presentation/attention-to-detail marks even though
the logic itself is correct.)*
