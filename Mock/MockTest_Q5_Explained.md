# Mock Test Question 5 Fully Explained From Zero

## Decoding the question, sentence by sentence

> "Task: Search Payment based on the email"

**Plain English:** This title is slightly misleading read carefully, the actual instructions
below ask you to search by **amount**, not email. (Titles/tasks in these mock questions sometimes
don't perfectly match the detailed bullet points always follow the detailed bullet points, as
they are what's actually graded.)

> "In the main code App.java (code in the link above), add a new method called searchPayments()
> to address below requirements: Asks the user to enter an amount."

**Plain English:** Create a brand-new method (doesn't exist yet in the given code you're adding
it entirely from scratch) called `searchPayments()`. Inside it, first ask the user to type in a
number (an amount).

> "Searches for all payments with that amount and prints the emails"

**Plain English:** Go through **every** `Payment` object that has been created so far (stored in
your `payments` array from Question 4), check if its payment amount matches what the user typed,
and if so, print that payment's email.

> "If there are no payments with the entered amount, the system should display a message saying
> that no payments were found"

**Plain English:** If you go through the whole array and find **zero** matches, print a message
saying so but only if there really were zero matches.

> "Write the code for searchPayments() method in the answer box below. You may refer to
> variables, methods, classes, or objects created in previous questions, to write
> searchPayments() method as required."

**Plain English:** You're allowed (and expected) to use the `payments` array, `paymentCount`,
and the `Payment` class's getters, all of which you built in Questions 3 and 4.

---

## Step 1 Setting up the method itself

```java
private static void searchPayments() {

}
```
- `private` only used internally within `App.java` (likely called from a menu option).
- `static` matches everything else in `App.java`.
- `void` this method's job is to **print** results directly, not hand back a value to
  whoever calls it.
- No parameters it will ask the user for the amount itself, using `Scanner`, rather than
  receiving it as an argument.

## Step 2 Asking the user for the amount to search for

```java
Scanner input = new Scanner(System.in);
System.out.print("Enter amount to search for: ");
int amount = input.nextInt();
```
(Using `int` here to match `pricePerRow`'s type from the given code, since prices in this project
are stored as whole-number `int` values, e.g. `50`, `80` if your `Payment` class stored the
amount as `double` instead, you'd use `input.nextDouble()` and compare as `double` always match
the type your `Payment.getPaymentAmount()` actually returns.)

## Step 3 The searching logic itself

This is the **exact same pattern** as searching any array of objects (from your Zero Part 5/7
lessons): loop through the *real* entries, compare a field, track whether anything matched.

```java
boolean found = false;

for (int i = 0; i < paymentCount; i++) {
    if (payments[i].getPaymentAmount() == amount) {
        System.out.println(payments[i].getEmail());
        found = true;
    }
}
```

Let's go through every part:
- `boolean found = false;` a flag, starting as "nothing found yet."
- `for (int i = 0; i < paymentCount; i++)` loops through indexes `0` up to (but not including)
  `paymentCount`. **This is critical**: it does NOT loop up to `100` (the array's full capacity).
  If it looped all the way to 100, and only (say) 3 payments actually exist so far, then indexes
  3 through 99 are still `null` (empty) calling `.getPaymentAmount()` on a `null` slot would
  crash the program with a `NullPointerException`. Looping only up to `paymentCount` guarantees
  you only ever touch real, existing `Payment` objects.
- `payments[i].getPaymentAmount()` gets the i-th payment out of the array, then calls its
  getter to read its stored amount.
- `== amount` compares that stored amount to what the user searched for. Since both are
  numbers, `==` is the correct comparison operator (recall: for comparing **Strings** like an
  email you'd need `.equals()` instead, never `==`).
- `System.out.println(payments[i].getEmail());` if it matched, print that specific payment's
  email using its getter.
- `found = true;` remembers a match was found, at least once.

## Step 4 Handling the "no matches" case

```java
if (!found) {
    System.out.println("No payments were found.");
}
```
This check happens **after** the loop has completely finished going through every payment. `!`
means "NOT" so `!found` reads as "found is NOT true" (i.e., it's still sitting at `false`,
meaning the loop above never once found a match). Only in that case do we print the "not found"
message.

**Why does this have to be a separate check after the loop, rather than inside the loop's
`else`?** If you tried to write an `else` attached to the `if` inside the loop, it would
incorrectly print "no payments found" for **every single payment that didn't match**, potentially
printing that message multiple times even when some payments DID match elsewhere in the array.
The flag pattern correctly waits until the whole array has been checked before deciding.

---

## Step 5 The complete final answer

```java
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
```

---

## Why this matters for the real exam

The real Question 5 will almost certainly ask for a very similar search method against the Car
Park project's array of objects (e.g. searching `Ticket`/`Vehicle` objects by fee, or by
registration number). **The pattern is identical every time:** loop up to the actual count (never
the full array capacity), compare the relevant field, use a `found` flag, and check that flag
only after the loop completes.

## Quick self-check
1. Why does the loop use `i < paymentCount` instead of `i < payments.length` (which would be 100)?
2. What Java error would occur if you looped all the way to 100 while only 5 real payments exist?
3. Why is the `found` flag checked only *after* the loop, rather than inside an `else` attached to
   the `if` inside the loop?
4. If `Payment`'s amount were stored as a `double` instead of `int`, what would need to change in
   this method?
5. Why is `==` the correct way to compare `amount` here, but it would be wrong to compare two
   emails this way?

*(1: because only `paymentCount` slots actually contain real objects the rest are still `null`
placeholders. 2: a `NullPointerException`, because calling a method like `.getPaymentAmount()` on
a `null` (non-existent) object is not allowed. 3: because putting it inside the loop's `else`
would cause the "not found" message to print once for every single non-matching payment checked,
even if a match was found elsewhere the flag correctly waits until the entire array has been
searched before making that decision. 4: you'd read the search value with `input.nextDouble()`
instead of `input.nextInt()`, and declare `double amount` instead of `int amount` everything
else stays the same. 5: `==` correctly compares numeric values directly; but for Strings, `==`
checks whether two variables point to the exact same object in memory, which can give incorrect
results even when the text content is identical `.equals()` is the correct way to compare the
actual text content of two Strings.)*
