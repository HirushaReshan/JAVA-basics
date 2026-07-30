# Library System — Task 5 Explained (`addStaff()` Method)

This task introduces a **new pattern** you haven't used before: adding an object to an array
**without** a counter variable, by searching for the first empty (`null`) slot instead. Pay close
attention — this is genuinely different from the Plane App's `Payment` approach, and is a common
point of confusion.

## Decoding the question, sentence by sentence

> "Declare (create) a new void method called addStaff()"

**Plain English:** Brand new method, doesn't exist yet, `void` return type (it performs actions —
prompting, creating, storing — but doesn't need to hand back a value).

> "a. Prompt user to enter staff details (name, surname and Staff ID)"

**Plain English:** Ask the user to type in all three pieces of information needed to build a
`Staff` object, one at a time.

> "b. Create an object from Staff type to store the details entered by user."

**Plain English:** Use the `Staff` constructor (from Task 3) to build a new `Staff` object using
what the user just typed.

> "c. Add the new object into the array you created in Task 04. If the array is full, print a
> message to inform user."

**Plain English:** Place the new object into `staffMembers[]`. **But you must first check if
there's actually room** — since Task 4 didn't ask for a counter, "is there room?" means "is there
still at least one `null` (empty) slot left in the array?" If every single slot already contains
a real `Staff` object (no `null` slots remain), print a message saying the list is full, and do
**not** try to add the new staff member anywhere (there's nowhere safe to put them).

---

## Step 1 — Gathering the input

```java
Scanner input = new Scanner(System.in);

System.out.print("Enter staff name: ");
String name = input.nextLine();

System.out.print("Enter staff surname: ");
String surname = input.nextLine();

System.out.print("Enter staff ID: ");
int staffID = input.nextInt();
```
Straightforward — `nextLine()` for the two text fields, `nextInt()` for the numeric ID, matching
the `Staff` constructor's expected parameter types exactly (`String, String, int`).

## Step 2 — Creating the object

```java
Staff staff = new Staff(name, surname, staffID);
```
Builds a new `Staff` object using the constructor from Task 3, passing in exactly what the user
typed, in the same order the constructor expects them.

## Step 3 — Finding a free slot (the new pattern)

Since there's no counter this time, you need to **loop through the array looking for the first
`null` slot** — the first position that doesn't yet contain a real object:

```java
boolean added = false;

for (int i = 0; i < staffMembers.length; i++) {
    if (staffMembers[i] == null) {
        staffMembers[i] = staff;
        added = true;
        break;
    }
}
```

Let's walk through this very carefully, since it's a new pattern:
- `boolean added = false;` — a flag, same idea as the `found` flag from your searching methods,
  but this time tracking whether we **successfully added** the new staff member.
- `for (int i = 0; i < staffMembers.length; i++)` — this time, we loop through the **entire**
  array capacity (`staffMembers.length`, which is 100), not just "up to a count" — because we
  genuinely don't know in advance which slots are filled and which are empty; we need to check
  every position to find the first free one.
- `if (staffMembers[i] == null)` — checks whether this specific slot is still empty. Note:
  comparing to `null` uses `==`, which is correct here (comparing an object reference to
  `null` is exactly what `==` is designed for — this is different from comparing the *content*
  of two Strings, which would need `.equals()`).
- `staffMembers[i] = staff;` — found an empty slot! Place the new object there.
- `added = true;` — remember that we succeeded.
- `break;` — **immediately stop the loop** — we don't need to keep searching once we've found and
  used one free slot; continuing would be wasted work (and could even overwrite our just-placed
  object with `null` logic mistakes if written carelessly).

## Step 4 — Reporting success or "array full"

```java
if (added) {
    System.out.println("Staff added successfully.");
} else {
    System.out.println("Staff list is full. Cannot add more staff.");
}
```
If the loop went through **every single slot** (all 100) and never found a `null` one, `added`
would still be `false` at this point — meaning every slot already has a real `Staff` object in
it, so there's genuinely no room, and we correctly inform the user instead of crashing or
silently failing.

---

## Step 5 — The complete final answer

```java
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
```

---

## Comparing this to the counter-based pattern (so you don't mix them up)

| | Counter-based (Plane App `Payment[]`) | Null-search-based (this `Staff[]` task) |
|---|---|---|
| Extra variable needed | `paymentCount` | None — just the array itself |
| Where to add next | `payments[paymentCount]` | First slot where `staffMembers[i] == null` |
| How to know "full" | `paymentCount == payments.length` | Loop finishes without ever finding a `null` slot |
| Loop bound when searching later | `i < paymentCount` | `i < staffMembers.length` (must check every slot, since gaps aren't tracked) |

**Why does this task use the null-search approach instead of a counter?** Simply because Task 4
didn't ask you to create a counter variable — so you must find another valid way to know where to
insert and whether the array is full. Both approaches are completely valid Java — always follow
what the specific question's earlier tasks actually gave you access to.

---

## Quick self-check
1. Why does this method loop through the *entire* array (`staffMembers.length`), rather than up
   to some counter?
2. Why is `==` correct for `staffMembers[i] == null`, when `.equals()` would be needed to compare
   two Strings?
3. What does `break;` do here, and why is it useful once a free slot has been found?
4. What would go wrong if you forgot the `break;` statement?
5. How would this method behave if called for the 101st time (after 100 staff already added)?

*(1: because without a counter, there's no other way to know which slots are already filled and
which are empty — every slot must be checked individually. 2: because `null` represents "no
object at all," and checking whether a variable currently refers to no object is exactly what
`==` is designed for; `.equals()` is for comparing the *content* of two actual objects, which
doesn't apply when one side is `null`. 3: it immediately stops the loop as soon as a free slot is
found and used, since there's no need to keep checking the remaining slots. 4: the loop would
keep running after already placing the new staff member, potentially overwriting it again with
itself in later iterations if additional null-check logic was written differently, and would
waste time checking already-known-filled slots — in this specific code it wouldn't cause a bug,
but it's inefficient and not good practice. 5: `added` would remain `false` after the loop
finishes (since all 100 slots are full), and the "Staff list is full" message would correctly
print instead of trying to add a 101st staff member.)*
