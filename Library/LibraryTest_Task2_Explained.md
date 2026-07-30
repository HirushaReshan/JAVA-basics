# Library System — Task 2 Explained (Validating Book Number)

This task introduces **two new ideas** you haven't been tested on yet in your other mock
questions: (1) validating that input is even a *number at all* (not just in range), and (2)
being told explicitly not to hardcode values. Let's go slowly.

## ⚠️ Assumed original `returnBook()` (lines 92-109), based on the established project pattern
```java
private static void returnBook() {
    Scanner input = new Scanner(System.in);
    System.out.print("Enter shelf number: ");
    int shelf = input.nextInt() - 1;
    System.out.print("Enter book number: ");
    int book = input.nextInt() - 1;

    if (bookShelves[shelf][book] == 1) {
        bookShelves[shelf][book] = 0;
        System.out.println("Book returned successfully.");
    } else {
        System.out.println("This book was not borrowed.");
    }
}
```

---

## Decoding the question, sentence by sentence

> "The book number must be an integer. If the user enters a non-integer value, display an error
> message and redirect user to the main menu."

**Plain English:** Right now, `input.nextInt()` **assumes** the user types a proper whole number.
If they type a word like `"abc"` instead, Java throws an exception (`InputMismatchException`) and
the program crashes. You must **catch** that possibility, show an error message instead of
crashing, and send the user back to the main menu (in practice: just stop this method early with
`return;` — since `runMenu()` is already a loop that shows the menu again automatically once a
method finishes).

> "If the user enters a valid integer as the book number, apply the following validations before
> the code related to returning a book: The book number cannot be smaller than 1 (note that the
> index starts at 0, not 1)."

**Plain English:** Once you know it's genuinely a number, THEN check its **range**. This sentence
is explicitly reminding you: the user types book numbers starting from 1 (human-friendly), but
the array itself starts at index 0 — so after you do `- 1` on the input, the valid lower bound
becomes `0`, not `1`. (Exact same translation logic as buyTicket()/parkCar() Question 2 in your
earlier mock tests.)

> "Book number cannot be larger than the number of books in the shelf selected (e.g., the maximum
> book number for shelf 1 is 10, the maximum book number for shelf 2 is 15, etc.)."

**Plain English:** The upper bound depends on **which shelf was chosen** — shelf 1 allows up to
book 10, shelf 2 up to book 15, because those shelves hold different numbers of books.

> "Validation should include the new row you have added in Question 1 (shelf 3)."

**Plain English:** Your check must automatically also correctly handle shelf 3 (10 books) without
you needing to write a separate, special case just for it.

> "Your solution should not contain hardcoded values."

**Plain English — this is the most important sentence in the whole task.** Do **NOT** write code
like this:
```java
if (book > 10) { ... } // ❌ HARDCODED — only correct for shelf 1!
```
A hardcoded value is a literal number typed directly into your condition that only happens to be
correct for one specific case. The problem: shelf 1 has 10 books, shelf 2 has 15, shelf 3 has 10
— if you hardcode `10`, your validation would be **wrong** for shelf 2 (letting invalid book
numbers 11-15 slip through undetected, or wrongly rejecting valid ones). Instead, you must ask
the array itself, dynamically, how many books are on the **currently selected shelf**:
```java
bookShelves[shelf].length // automatically the correct max for WHICHEVER shelf was picked
```
This single expression works correctly for shelf 1 (returns 10), shelf 2 (returns 15), AND shelf
3 (returns 10) — with zero extra code, because you're asking the array for its real length
instead of assuming a fixed number. This is precisely why the earlier task (Task 1) matters: as
long as shelf 3 was created correctly with the right size, this validation automatically extends
to cover it too — nothing extra needed here.

---

## Step 1 — Handling the "must be an integer" check with try/catch

Recall from Zero-knowledge Part 6: `try/catch` lets you attempt risky code and recover gracefully
instead of crashing.

```java
System.out.print("Enter book number: ");
int book;
try {
    book = input.nextInt();
} catch (Exception e) {
    System.out.println("Invalid input. Book number must be an integer.");
    return; // exits returnBook() immediately, sending control back to runMenu()'s loop
}
book = book - 1; // now safely convert to zero-indexed, AFTER confirming it was a real integer
```

**Why declare `int book;` without a value, then assign inside the `try`?** Because `book` needs
to still exist (be usable) both inside the `try` block AND potentially afterward, but its value
can only successfully be set if `input.nextInt()` doesn't throw an exception. Declaring it once,
outside the `try`, and assigning it inside, is the standard pattern for this situation.

**Why `return;` inside the `catch`?** Since `returnBook()` is `void`, a bare `return;` immediately
exits the method — nothing below it runs. Because `runMenu()`'s `while` loop calls `getOption()`
and re-displays the menu every time a method finishes, exiting `returnBook()` early effectively
**is** "redirecting the user to the main menu," exactly as required.

---

## Step 2 — Adding the range validations, using dynamic lengths (no hardcoding)

```java
if (book < 0 || book >= bookShelves[shelf].length) {
    System.out.println("Invalid book number.");
} else if (bookShelves[shelf][book] == 1) {
    bookShelves[shelf][book] = 0;
    System.out.println("Book returned successfully.");
} else {
    System.out.println("This book was not borrowed.");
}
```
- `book < 0` — catches the "cannot be smaller than 1" rule, translated to zero-indexed terms
  (human "1" becomes code "0" after the `-1` conversion, so the lower valid bound is `0`).
- `book >= bookShelves[shelf].length` — catches the "cannot be larger than the shelf's book
  count" rule, **dynamically**, using the actual array length of whichever shelf was selected —
  this single condition is automatically correct whether `shelf` refers to Shelf 1 (10 books),
  Shelf 2 (15 books), or Shelf 3 (10 books), with no extra code needed for shelf 3.

  Note: we use `>=`, not `>`. Why? If a shelf has 10 books (indexes 0-9), then index `10` is
  **already invalid** (out of bounds) — `book >= 10` correctly catches this. Using `book > 10`
  would wrongly allow `book == 10` through, which doesn't actually exist.

---

## Step 3 — The complete final answer

```java
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
```

*(This example doesn't add shelf-number validation, since the task specifically only asked for
book-number validation — but if your real exam question also asks you to validate `shelf`, use
the identical dynamic-bounds pattern: `shelf < 0 || shelf >= bookShelves.length`, again using
`.length` instead of hardcoding `3`.)*

---

## Quick self-check
1. Why must the `try/catch` for the integer check happen **before** the `- 1` conversion and
   range checks, not after?
2. Give an example of a hardcoded value this task specifically warns you against, and explain why
   it would be wrong.
3. Why does `bookShelves[shelf].length` automatically work correctly for all three shelves,
   without any extra code for shelf 3?
4. Why is `>=` used instead of `>` when checking the upper bound?
5. Why does a plain `return;` inside the `catch` block correctly satisfy "redirect the user to
   the main menu"?

*(1: because you can't safely do arithmetic like `-1` or compare bounds on a value that might not
even be a valid number yet — you must first confirm it's a real integer, or the `-1`/comparison
code itself could throw a different, unhandled crash. 2: writing `if (book > 10)` — this is only
correct for shelves with exactly 10 books; it would silently allow invalid book numbers on shelf
2 (which has 15) through undetected. 3: because `.length` asks the actual array for its real,
current size at runtime, rather than assuming a fixed number — whichever shelf's array was
created with whatever size, `.length` always reports it correctly. 4: because valid indexes only
go up to `length - 1`; using `book > length` would wrongly allow the invalid index equal to
`length` itself through. 5: because `returnBook()` is void, and `return;` on its own immediately
ends the method with no output value needed — since `runMenu()`'s loop then automatically shows
the menu again, this achieves the same effect as "going back to the main menu.")*
