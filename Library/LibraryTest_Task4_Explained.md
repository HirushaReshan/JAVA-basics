# Library System — Task 4 Explained (Creating the `Staff` Object Array)

## Decoding the question, sentence by sentence

> "In the main file (App.java), create a new standard array of objects of type Staff as a global
> variable (after line number 3 in App.java file). Do not use ArrayList."

**Plain English:** Add one line to `App.java`, declaring a global array field that will hold
`Staff` objects. "Standard array" and "Do not use ArrayList" is the exam explicitly ruling out an
alternative, more advanced Java tool called `ArrayList` (a resizable list class you haven't been
taught yet) — they want the plain `Type[] name = new Type[size];` syntax you already know from
every other array in this module (Zero-knowledge Part 4).

"After line number 3" simply tells you **where** in the file to place this new line — right near
the top, alongside the other global variables like `bookShelves`, matching the same style as
`payments`/`tickets` arrays in your earlier mock questions.

> "Initialize the array with size 100."

**Plain English:** Same pattern as `Payment[100]` from your Plane App Question 4 — room for 100
`Staff` objects.

---

## Step 1 — Recognising this uses the exact same array-of-objects pattern you already know

From Zero-knowledge Part 5 and your Plane App Question 4:
```java
private static Payment[] payments = new Payment[100];
```
This task asks for the identical structure, just with `Staff` instead of `Payment`:
```java
private static Staff[] staffMembers = new Staff[100];
```

## Step 2 — Why "Do not use ArrayList" matters

You might have seen `ArrayList<Staff>` in some other contexts online — this is a different,
**more advanced** Java tool that can grow and shrink automatically, with built-in methods like
`.add()` and `.remove()`. Your module has **not** covered `ArrayList` (LO1-LO5 for this module
focus on plain arrays), so the exam is explicitly protecting you from accidentally using a tool
you haven't been taught, and confirming they want the fixed-size array approach instead.

```java
// ❌ NOT what's being asked for:
ArrayList<Staff> staffMembers = new ArrayList<Staff>();

// ✅ What's actually being asked for:
private static Staff[] staffMembers = new Staff[100];
```

## Step 3 — Do you need a counter variable too?

**Notice this task does NOT mention a counter** (unlike the Plane App's `paymentCount`). This is
a deliberate difference you'll need to handle in Task 5 — instead of tracking a count, you'll
need to search the array for the first *empty* (`null`) slot when adding a new staff member. This
is covered fully in the Task 5 explanation file — for now, Task 4 only asks for the array itself.

---

## Step 4 — The complete final answer

```java
private static Staff[] staffMembers = new Staff[100];
```

That's it — just one line, placed near the top of `App.java`, alongside your other global
variables like `bookShelves`.

---

## Quick self-check
1. Why does the question explicitly say "Do not use ArrayList"?
2. What value does every slot in `new Staff[100]` hold immediately after this line runs, and why?
3. Why doesn't this task ask you to also create a counter variable, unlike the Plane App's
   `Payment[]` task?
4. What would `staffMembers[0]` currently be, right after this line runs, before you've added any
   staff?

*(1: because `ArrayList` is a more advanced, resizable collection type not covered in this
module — the exam wants you to use the plain fixed-size array syntax you've actually been taught.
2: `null`, because arrays of objects (not primitive types like `int`) default every slot to
`null` — meaning "no object here yet" — until you explicitly create and place real objects into
them. 3: because this task's approach for finding an empty slot to add to (in Task 5) will search
for `null` slots directly, rather than tracking a running count — a valid alternative pattern to
the counter approach used elsewhere. 4: `null`, since no `Staff` object has been created and
placed there yet.)*
