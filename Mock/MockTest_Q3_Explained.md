# Mock Test Question 3 — Fully Explained From Zero

## Decoding the question, sentence by sentence

> "Task: Add a new class to the project to satisfy requirements below. (Assume this class will
> be in a separate file called Payment.java)"

**Plain English:** You're not editing `App.java` at all in this question. You're creating a
**brand new file**, and inside it, a **brand new class** called `Payment`. Remember from your
Zero-knowledge Part 5: a class is a blueprint describing what data (fields) and abilities
(methods) something will have.

> "1. Create a new class called Payment"

**Plain English:** Literally: `public class Payment { ... }`

> "2. This class should have the following attributes with the correct declaration: Email,
> Payment amount"

**Plain English:** "Attributes" = "fields" (used interchangeably — both mean the class's own
variables that each object will hold). You need **two fields**:
- One to store an email (this is text → `String` type)
- One to store a payment amount (this is a number that could have decimals, like money → `double`
  type)

"With the correct declaration" means: correct **access modifier** (should be `private`, following
encapsulation — see Zero Part 5) and correct **data type** for each.

> "3. Add a constructor to the class that takes the two attributes as input."

**Plain English:** Write a constructor (same name as the class, no return type) that accepts
both the email and the payment amount as parameters, and stores them into the object's fields.

> "4. The new class should have the following getters and setters: getEmail() and setEmail(),
> getPaymentAmount() and setPaymentAmount()"

**Plain English:** For each of your two fields, write one getter (returns the current value) and
one setter (changes the value) — following Java's exact naming convention: `get` + field name
(capitalised), and `set` + field name (capitalised).

> "5. Add the following method, which prints on the output window the email and the payment
> amount: printPayment()"

**Plain English:** Write one more method, `void printPayment()`, that uses
`System.out.println(...)` to display both pieces of information together.

---

## Step 2 — Building the class piece by piece

### The class declaration
```java
public class Payment {

}
```
`public` so `App.java` (a different class) is allowed to use it. `class` declares it. `Payment`
is the name — and remember, this MUST match the filename exactly: `Payment.java`.

### The fields (attributes)
```java
private String email;
private double paymentAmount;
```
- `private` — encapsulation: outside code cannot touch these directly, only through the
  getters/setters you'll write below.
- `String email;` — text data type, correctly matches "Email" being a piece of text.
- `double paymentAmount;` — a decimal number type, correctly matches "Payment amount" needing to
  represent money values like `50.0` or `79.99`.

**Why is choosing the correct type part of "correct declaration"?** If you used `int
paymentAmount`, you couldn't ever store a value like `79.99` — it would be forced to round to a
whole number, losing accuracy. The exam is specifically testing whether you pick sensible types
for real-world data.

### The constructor
```java
public Payment(String email, double paymentAmount) {
    this.email = email;
    this.paymentAmount = paymentAmount;
}
```
- `public Payment(...)` — name matches class exactly, no return type at all (not even `void`) —
  this is what makes it a constructor.
- `(String email, double paymentAmount)` — the constructor takes exactly the two attributes
  mentioned in the question, "as input" (as parameters).
- `this.email = email;` — takes the parameter `email` (just passed in) and stores it into
  **this object's own** `email` field. The `this.` is required here because the parameter and
  field share the same name — without it, Java can't tell them apart, and the assignment would
  silently do nothing (covered in depth in Zero Part 5).

### The getters
```java
public String getEmail() {
    return email;
}
public double getPaymentAmount() {
    return paymentAmount;
}
```
- Naming: `get` + field name, first letter capitalised. `email` → `getEmail`. `paymentAmount` →
  `getPaymentAmount`.
- No parameters — a getter just hands back what's already stored.
- Return type matches the field's type exactly (`String` for email, `double` for the amount).

### The setters
```java
public void setEmail(String email) {
    this.email = email;
}
public void setPaymentAmount(double paymentAmount) {
    this.paymentAmount = paymentAmount;
}
```
- Naming: `set` + field name.
- Always `void` — a setter changes something, it doesn't hand anything back.
- Takes exactly one parameter: the new value to store.
- Uses `this.` for the same reason as the constructor.

### The print method
```java
public void printPayment() {
    System.out.println("Email: " + email + ", Payment Amount: " + paymentAmount);
}
```
- `void` because it just performs an action (printing), returning nothing.
- No parameters needed — it uses the object's own current field values.
- `+` here is **String concatenation**: joining pieces of text together. When you use `+` next
  to a String and a non-String value (like `paymentAmount`, a `double`), Java automatically
  converts the number into its text form so it can be joined in.

---

## Step 3 — The complete final answer

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

## How you'd actually use this class (not asked in Q3, but helps understanding)
```java
Payment p = new Payment("alex@email.com", 50.0);
p.printPayment(); // Email: alex@email.com, Payment Amount: 50.0
System.out.println(p.getEmail()); // alex@email.com
p.setPaymentAmount(80.0);
System.out.println(p.getPaymentAmount()); // 80.0
```

---

## Why this matters for the real exam

The real exam's Question 3 will ask for a similarly structured class (maybe called `Ticket` or
`Vehicle`), with two different-but-similar fields (e.g. a registration number and a fee instead
of an email and a payment amount). **The entire shape of the answer is identical** — 2 private
fields with correct types, a constructor using `this.`, matching getters/setters, and one print
method. Master this exact structure and you can produce it for any field names/types they give
you.

## Quick self-check
1. Why must the constructor have no return type at all, not even `void`?
2. What data type would you choose for a field representing a car registration plate, and why?
3. What bug happens if you write `email = email;` instead of `this.email = email;` in the
   constructor?
4. Why does the setter's return type have to be `void`, while the getter's return type must match
   the field?
5. Why is `paymentAmount` declared as `double` and not `int`?

*(1: because that's exactly what makes it a constructor rather than a regular method — Java
identifies it by name-matching-the-class and having no return type. 2: `String`, because a
registration plate contains letters and numbers together as text, not a pure numeric value you'd
do maths on. 3: the field never actually gets updated — the parameter is assigned to itself,
so the object's real `email` field stays at its default value (`null`) forever. 4: a setter
just changes something and hands nothing back (`void`); a getter's entire job is to hand back
the field's value, so its return type must match that field's type exactly. 5: because payment
amounts commonly include decimal places (like £79.99), and `int` can only store whole numbers.)*
