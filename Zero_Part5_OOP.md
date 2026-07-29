# Java From Absolute Zero  Part 5: Object-Oriented Programming (OOP)

This is the **most important part** for your exam (Questions 3 and 4 are entirely about this).
Take your time here.

---

## 1. What problem does OOP solve?

Imagine you're tracking payments: each one has an email and an amount. You *could* use two
separate arrays:
```java
String[] emails = new String[100];
double[] amounts = new double[100];
```
But this is fragile  nothing stops `emails[5]` and `amounts[7]` from accidentally referring to
two *different* people's data (a mismatch). **OOP solves this by bundling related data (and the
actions on that data) together into one unit, called an object.**

---

## 2. Class vs Object  the single most important distinction

- A **class** is a **blueprint/template**. It defines *what information* and *what abilities*
  something will have  but it is not, itself, a real "thing" you can use yet.
- An **object** is an actual **instance** created from that blueprint  a real, usable "thing" in
  memory, with its own actual values filled in.

**Analogy:** `class Car` is like an architectural blueprint for a car  it defines "a car has a
colour and a speed, and can accelerate." An actual `Car` object is a specific car built from that
blueprint  e.g. "a red car currently going 60mph." You can build many objects (many actual cars)
from the same one blueprint (class).

```java
public class Car {           // the BLUEPRINT (class)
    private String colour;
    // ...
}

Car myCar = new Car("red");   // an actual OBJECT, built using the blueprint
Car yourCar = new Car("blue"); // a DIFFERENT object, same blueprint, different data
```

---

## 3. Writing a class  every single piece explained

```java
public class Ticket {

    // ---- FIELDS (also called attributes / instance variables) ----
    private String registrationNumber;
    private double parkingFee;

    // ---- CONSTRUCTOR ----
    public Ticket(String registrationNumber, double parkingFee) {
        this.registrationNumber = registrationNumber;
        this.parkingFee = parkingFee;
    }

    // ---- GETTERS ----
    public String getRegistrationNumber() {
        return registrationNumber;
    }
    public double getParkingFee() {
        return parkingFee;
    }

    // ---- SETTERS ----
    public void setRegistrationNumber(String registrationNumber) {
        this.registrationNumber = registrationNumber;
    }
    public void setParkingFee(double parkingFee) {
        this.parkingFee = parkingFee;
    }

    // ---- OTHER METHODS ----
    public void printTicket() {
        System.out.println("Registration: " + registrationNumber + ", Fee: £" + parkingFee);
    }
}
```

### 3a. Fields (attributes)
```java
private String registrationNumber;
private double parkingFee;
```
These are **variables that live inside the class**, representing the actual data each object
will hold. They are declared just like normal variables (type + name), but written directly
inside the class, not inside any specific method.

**Why `private`?** This is called **encapsulation**  one of the core ideas of OOP. By making
fields `private`, no code *outside* this class can directly do `someTicket.parkingFee = -999;`
and corrupt the data. Instead, outside code is **forced** to go through your controlled
getter/setter methods, which you could later add validation to if needed (e.g. "reject negative
fees").

### 3b. The Constructor
```java
public Ticket(String registrationNumber, double parkingFee) {
    this.registrationNumber = registrationNumber;
    this.parkingFee = parkingFee;
}
```
A **constructor** is a special method that runs **automatically** the moment you create a new
object with `new`. Its job is to set up the object's initial data.

Rules that make it a constructor (not a normal method):
1. Its name must be **exactly the same** as the class name (`Ticket`).
2. It has **no return type at all**  not even `void`. (This is different from every other
   method you'll write.)

**Why do the parameter names match the field names?** It's completely normal and expected style
 `registrationNumber` (the field) and `registrationNumber` (the parameter) are allowed to share
a name. This is exactly what `this.` is for (see below)  it lets Java tell them apart.

**Calling a constructor (creating an object):**
```java
Ticket t = new Ticket("AB12 CDE", 5.0);
```
- `new`  "actually build a real object in memory now."
- `Ticket(...)`  calls the constructor, matching it by the number/type of arguments you give.
- `"AB12 CDE"` and `5.0`  the arguments, which get copied into the constructor's parameters
  `registrationNumber` and `parkingFee`.
- `Ticket t = ...`  stores a reference to this new object in a variable called `t`, of type
  `Ticket`.

### 3c. The `this` keyword  explained very precisely
`this` always means **"the specific object that this method/constructor is currently running
for."** Inside the constructor:
```java
this.registrationNumber = registrationNumber;
//   ^ the FIELD                ^ the PARAMETER (just passed in)
```
Read this line as: *"Take the value that was just passed into the parameter called
`registrationNumber`, and store it into THIS object's own field, which is also (confusingly, but
validly) named `registrationNumber`."*

**What if you forgot `this.`?**
```java
public Ticket(String registrationNumber, double parkingFee) {
    registrationNumber = registrationNumber; // BUG!
    parkingFee = parkingFee;                  // BUG!
}
```
Without `this.`, Java assumes both sides refer to the **parameter** (since it's the "closest"
matching name). So this line is really saying "take the parameter and assign it... to itself,"
which does absolutely nothing useful. The object's actual field is **never updated**, and stays
at its default value (`null` for `registrationNumber`, `0.0` for `parkingFee`) forever. This is
one of the single most common beginner bugs in exactly this kind of exam question.

### 3d. Getters
```java
public String getRegistrationNumber() {
    return registrationNumber;
}
```
A getter is simply a method that **returns the current value** of a private field, so outside
code can read it (since it can't access the private field directly).
- Naming convention: `get` + the field name, with the first letter capitalised
  (`registrationNumber` → `getRegistrationNumber`).
- Return type matches the field's type exactly.
- Takes **no parameters**  it just hands back what's already stored.

### 3e. Setters
```java
public void setRegistrationNumber(String registrationNumber) {
    this.registrationNumber = registrationNumber;
}
```
A setter **changes** the value of a private field from outside code.
- Naming convention: `set` + field name.
- Return type is always `void` (it doesn't give anything back  it just updates something).
- Takes **one parameter**: the new value to store.
- Uses `this.` for exactly the same reason as the constructor.

**Using getters and setters:**
```java
Ticket t = new Ticket("AB12", 5.0);
System.out.println(t.getRegistrationNumber()); // "AB12"
t.setParkingFee(8.0);                            // change the fee
System.out.println(t.getParkingFee());          // 8.0
```

### 3f. A regular instance method (like `printTicket()`)
```java
public void printTicket() {
    System.out.println("Registration: " + registrationNumber + ", Fee: £" + parkingFee);
}
```
This is just a normal method living inside the class. It's **not static**, so it must be called
on a specific object (`t.printTicket();`), and inside it, `registrationNumber` and `parkingFee`
automatically refer to **that specific object's** own field values.

---

## 4. Static vs Instance  the difference that trips people up most

| | **Static** | **Instance (non-static)** |
|---|---|---|
| Belongs to | The class itself (shared) | Each individual object (separate copy per object) |
| Called via | `ClassName.method()` | `objectVariable.method()` |
| Can access instance fields directly? | ❌ No | ✅ Yes |
| Example in your project | Everything in `App.java` | Everything in `Ticket`/`Vehicle`/`Payment` |

```java
Ticket t1 = new Ticket("AB12", 5.0);
Ticket t2 = new Ticket("CD34", 8.0);

t1.getParkingFee(); // 5.0  belongs to t1
t2.getParkingFee(); // 8.0  belongs to t2, completely separate from t1
```
Each object has its **own independent copy** of every instance field. This is the entire reason
OOP is powerful for something like the exam: you can have 100 separate `Ticket` objects, each
remembering its own registration number and fee, without any risk of mixing up whose data belongs
to whom (unlike the fragile parallel-arrays approach from Section 1).

**Rule for your exam:** `App.java` → everything `static` (because `main` is static and needs
direct access). Your new class (`Payment`/`Ticket`/`Vehicle`) → fields and methods **not**
`static` (normal OOP objects).

---

## 5. Putting it all together  creating and using objects from `App.java`

```java
// Inside App.java (static context)
private static Ticket[] tickets = new Ticket[100]; // array that WILL hold Ticket objects
private static int ticketCount = 0;                 // how many are actually filled in

private static void parkCar() {
    // ... after successfully parking ...
    Ticket ticket = new Ticket("AB12 CDE", 5.0); // CREATE a new object
    tickets[ticketCount] = ticket;                // STORE it in the array
    ticketCount++;                                 // update the counter
}
```
Step by step: `new Ticket(...)` builds a brand-new `Ticket` object in memory, running its
constructor, and hands back a reference to it, which we store first in a local variable `ticket`,
then place into the array at position `ticketCount`. Finally we increase `ticketCount` by one so
we know there's now one more real object stored.

---

## 6. Access Modifiers  full recap in the OOP context

| Modifier | On a field | On a method | On a class |
|---|---|---|---|
| `private` | Only this class can read/change it directly | Only this class can call it | *(not usually used on top-level classes)* |
| `public` | Any code, anywhere | Any code, anywhere | Any code, anywhere can use this class |

**The standard, expected exam pattern:**
- All fields → `private` (encapsulation).
- Constructor → `public` (so `App.java` can create objects with `new Ticket(...)`).
- Getters/setters → `public` (so `App.java` can read/change the private data safely).
- Print methods → `public`.

---

## Quick Self-Test
1. What is the difference between a class and an object, in your own words?
2. Why must a constructor have no return type, not even `void`?
3. What bug happens if you forget `this.` inside a setter?
4. Why are fields made `private` if you're just going to expose them again with public getters/setters?
5. Why can't `App.java`'s static `main()` method directly use an instance field from `Ticket`
   without first creating a `Ticket` object?

**Next: Part 6  Exceptions and File I/O**
