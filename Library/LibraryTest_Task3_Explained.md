# Library System — Task 3 Explained (The `Staff` Class)

This follows the exact same pattern as your `Payment` class (Plane App Q3), with one new twist:
you must **explicitly write a comment explaining encapsulation**, not just apply it silently.

## Decoding the question, sentence by sentence

> "Create a new class called Staff... attributes: Name, Surname, StaffID – this should be
> number"

**Plain English:** Three fields: two pieces of text (`String`), and one number. "This should be
number" specifically tells you `StaffID` must be a numeric type — `int` is the natural choice
(IDs are whole numbers, no decimals needed).

> "Add a constructor to this class which takes the 3 values similar to attributes, as input."

**Plain English:** One constructor, taking all 3 fields as parameters — identical pattern to
`Payment`'s constructor, just with 3 parameters instead of 2.

> "Add the following getters and setters... getID() and setID()"

**Plain English:** ⚠️ **Watch this carefully** — the field is called `StaffID`, but the getter/
setter are named `getID()`/`setID()`, **not** `getStaffID()`/`setStaffID()`. This is a
deliberate exam trap testing whether you blindly follow the "get + field name" pattern without
actually reading what name they specified. Always use the **exact method names given in the
question**, even if they don't perfectly match the field name.

> "Add the following void method, which prints the values of the 3 attributes: printStaff()"

**Plain English:** Same as `printPayment()` before — one `void` method, no parameters, prints all
3 fields via `System.out.println`.

> "Include relevant modifications to the Staff class to ensure it follows the encapsulation
> principle. At the end of the class, include a brief comment explaining how encapsulation has
> been applied within the Staff class."

**Plain English:** Two separate things here:
1. **The "modification"**: make sure all 3 fields are declared `private` (this is what actually
   *implements* encapsulation — nothing new code-wise beyond what you'd already do).
2. **The comment**: literally write a `//` comment at the bottom of the file, in plain English,
   explaining *why* what you did counts as encapsulation. This is new — none of your earlier
   mock questions asked you to write an explanatory comment as a graded deliverable.

---

## Step 1 — What is "encapsulation," precisely? (so you can explain it correctly)

**Encapsulation** = hiding an object's internal data (fields) from direct outside access, and
only allowing controlled access through public methods (getters/setters). The three ingredients
that make a class "encapsulated":
1. Fields are declared `private`.
2. Public getter methods allow *reading* the data.
3. Public setter methods allow *changing* the data — and could include validation logic if
   needed (e.g. rejecting a negative StaffID), even though this basic version doesn't add extra
   checks.

---

## Step 2 — Building the class

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

    // Encapsulation has been applied in this class by declaring all attributes (name, surname,
    // staffID) as private, meaning they cannot be accessed directly from outside the class.
    // Instead, access is only possible through the public getter and setter methods
    // (getName/setName, getSurname/setSurname, getID/setID), which control how the data can be
    // read or changed, protecting the internal state of each Staff object.
}
```

## Step 3 — Walking through every part

- `private String name; private String surname; private int staffID;` — all three fields are
  `private`, satisfying the encapsulation requirement at the data level.
- `public Staff(String name, String surname, int staffID)` — constructor name matches the class,
  no return type, takes all 3 values as parameters, exactly "similar to attributes" as worded.
- `this.name = name;` etc. — standard pattern: `this.` refers to the current object's own field,
  distinguishing it from the parameter of the same name.
- `getName()`/`setName()`, `getSurname()`/`setSurname()` — follow the normal "get/set + field
  name" convention, since the question named them normally for these two.
- `getID()`/`setID()` — **do NOT** write `getStaffID()` here; the question explicitly specified
  `getID()`/`setID()` as the exact names required, even though the field itself is `staffID`.
  Inside the method, you still return/assign the real `staffID` field — only the **method name**
  differs from what you might expect.
- `printStaff()` — `void`, no parameters, prints all three current field values in one line.
- The comment at the end — plain English, explains *what* was done (fields are private) and
  *why* it matters (outside code can only interact through public getters/setters, protecting
  the object's internal state).

---

## Quick self-check
1. Why is `staffID` declared as `int` rather than `String`?
2. Why must the getter/setter for the ID be named `getID()`/`setID()` and not
   `getStaffID()`/`setStaffID()`?
3. What are the three ingredients that make a class properly encapsulated?
4. Why does the comment go at the **end** of the class, and why must it be a comment (`//`) and
   not a `System.out.println` statement?

*(1: because "this should be number" explicitly specifies a numeric type, and IDs are whole
numbers with no need for decimals, making `int` the correct choice. 2: because the question
explicitly names the required method names — exam questions sometimes intentionally don't match
the "obvious" naming convention, to test whether you follow the exact instructions rather than
assuming a pattern. 3: private fields, public getters for reading, public setters for writing/
changing. 4: it goes at the end because the question specifically says "at the end of the class";
it must be a `//` comment (not printed output) because it's documentation for a human reader
examining your code, not something meant to run or display to a program user.)*
